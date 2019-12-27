---
layout: post
title: ECS 배포된 Django 서버의 Health check 문제해결법
description: "Trouble shooting django health check in ecs & alb"
tags: [AWS, ECS]
---

## 개요

ECS를 통해 Django server를 배포하고 로드밸런서를 연결하고, ECS instance(EC2)를 대상그룹을 통해 연결시켜놓았다.  
그런데 등록된 인스턴스에 대한 Health check를 대상그룹에 설정해놓았는데 계속 상태체크가 실패하여 인스턴스 유지가 되지 않는 문제가 발생하였다.

## 원인

Health check 의 경우 도메인이 아닌 인스턴스의 IP로 호출하고 있었다. 인스턴스는 Auto scaling 그룹에 의해 동적으로 생성되고 있어 미리 설정할 수도 없었다.

## Solution

- [EC2 인스턴스 메타데이터 문서](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)
- [AWS EC2 리눅스 인스턴스 식별 문서](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/identify_ec2_instances.html)
  위 문서를 참조하면 서버 시작시 인스턴스의 메타정보를 가져와서 셋팅할 수 있다.

1. 메타정보를 가져올 수 있게 준비한다

```python
def has_ec2_hypervisor_uuid():
    if os.path.isfile('/sys/hypervisor/uuid'):
        with open('/sys/hypervisor/uuid') as f:
            uuid = f.read()
            return uuid.startswith('ec2')
    return False

def get_linux_ec2_private_ip():
    from urllib.request import urlopen
    if not has_ec2_hypervisor_uuid():
        return None
    try:
        response = urlopen('http://169.254.169.254/latest/meta-data/local-ipv4')
        ec2_ip = response.read().decode('utf-8')
        if response:
            response.close()
        return ec2_ip
    except Exception as e:
        print(e)
        return None
```

2. Django `settings` 에서 설정한다.

```python
# ...
PRIVATE_HOST_IP = get_linux_ec2_private_ip()

ALLOWED_HOSTS = [
    # ...
]
if PRIVATE_HOST_IP:
    ALLOWED_HOSTS.append(PRIVATE_HOST_IP)
# ...
```

- 주의사항
  - 위의 `has_ec2_hypervisor_uuid` 의 경우에는 t3 타입에서는 올바른 값을 반환하지 못하는 문제가 있었다.

3. t3 타입에서 사용가능한 방법

```python
def has_ec2_hypervisor_uuid():
  # ...

def has_ec2_dmi_uuid():
    try:
        uuid = subprocess.check_output(
            ['cat', '/sys/devices/virtual/dmi/id/product_uuid'],
            stderr=subprocess.STDOUT,
        ).decode('utf-8')
        return uuid.startswith('EC2')
    except subprocess.CalledProcessError:
        return False

def get_linux_ec2_private_ip():
    # T2/T3 모두 대응할 수 있게 함
    from urllib.request import urlopen
    if not (has_ec2_dmi_uuid() or has_ec2_hypervisor_uuid()):
        return None
    try:
        response = urlopen('http://169.254.169.254/latest/meta-data/local-ipv4')
        ec2_ip = response.read().decode('utf-8')
        if response:
            response.close()
        return ec2_ip
    except Exception as e:
        print(e)
        return None

```
