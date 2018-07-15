---
layout: post
title: ElasticSearch의 설치와 기초 명령어
description: "ElasticSearch installation, crud"
tags: [ElasticSearch, Installation]
---

## 설치 (Ubuntu 16.04)
- Elastic search 다운로드
  - https://www.elastic.co/kr/downloads/elasticsearch

```bash
$ sudo dpkg -i elasticsearch-x.x.x.deb

# Setup
$ sudo systemctl enable elasticsearch.service

# Start & Stop
$ sudo service elasticsearch start
$ sudo service elasticsearch stop

# 가동확인
$ sudo curl -XGET localhost:9200
{
  "name" : "YXOiPYm",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "DmLghlP0SkyvTTLGT7A7Vg",
  "version" : {
    "number" : "6.3.1",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "eb782d0",
    "build_date" : "2018-06-29T21:59:26.107521Z",
    "build_snapshot" : false,
    "lucene_version" : "7.3.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}

# Config loc
/etc/elasticsearch
# Init script
/etc/init.d/elasticsearch
```

## 데이터구조
> Index > Type > Document

- 관계형 DB와의 비교
| ES | RDB | 
| --- | --- |
| Index | Database |
| Type | Table |
| Document | Row |
| Field | Column |
| Mapping | Schema|

- ES에서는 REST API가 SQL문을 대신함

## 기초 조작
- 인덱스 생성 & 삭제

```bash
# 생성
$ curl -XPUT http://localhost:9200/[Index명]
{"acknowledged":true,"shards_acknowledged":true,"index":"Index명"}

# 삭제
$ curl -XDELETE http://localhost:9200/[Index명]
{"acknowledged":true}
```

- Document 생성
  - Index 없으면 같이 생성해줌

```bash
# 데이터 직접생성
$ curl -XPOST http://localhost:9200/[Index명]/[Document명]/1/ -H 'Content-Type:application/json' -d '{"Key": "Value"}'

# 데이터 파일로부터 생성
$ curl -XPOST http://localhost:9200/[Index명]/[Document명]/1/ -H 'Content-Type:application/json' -d @[파일명].json
```

- 업데이트

```bash
$ curl -XPOST http://localhost:9200/[Index명]/[Document명]/1/ -H 'Content-Type:application/json' -d '{"Key": "Value"}'
```

- Bulk insert

```bash
$ curl -XPOST http://localhost:9200/_bulk? -H 'Content-Type:application/json' --data-binary @[파일명].json
```

### Mapping
- RDS 의 Schema에 해당
- Mapping 없이 넣는것도 가능하나 잘못된 데이터의 입력을 방지하기 위해 필요함.
- 6.x 버전에서는 string -> text 으로 수정

```bash
$ curl -XPUT http://localhost:9200/[Index]/[Document]/_mapping? -H 'Content-Type:application/json' -d @[Mapping데이터파일명].json
```