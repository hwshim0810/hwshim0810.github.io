---
layout: post
title: REST API
description: "REST API"
tags: [Web, REST API]
modified: 2017-04-12
---
# REST(Representational State Transfer)란?

## REST 의 구성
- **자원(Resource) : URI**
- **행위(Verb) : HTTP Method**
- **표현(Representations)**

## 특징
1. Uniform(Uniform Interface)
- URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍쳐
2. Stateless(무상태성)
- REST는 무상태성 성격 : 작업을 위한 상태정보를 따로 저장하고 관리하지 않는다
- 세션/쿠키정보를 별도로 저장/관리하지 않기 때문에 API서버는 들어오는 요청을 단순처리
따라서 서비스의 자유도가 높아지고 불필요한 정보관리가 없어 구현이 단순해짐
3. Cacheable(캐시가능)
- REST는 HTTP라는 기존 웹표준을 그대로 사용하기 떄문에 웹의 기존 인프라 활용가능
- HTTP의 캐싱기능 : Last-Modified 태그나 E-Tag를 이용하여 캐싱구현
4. Self-descriptiveness(자체표현구조)
- REST API 메세지만 보고도 쉽게 이해 할 수 있는 자체표현구조
5. Client-Server구조
- REST 서버는 API제공
- 클라이언트는 사용자 인증이나 컨텍스트(세션/로그인정보)등을 직접 관리하는 구조
- 구분이 명확, 의존성을 줄임
6. 계층형 구조
- 서버는 다층 계층으로 구성될 수 있으며 보안, 로드벨런싱, 암호화계층을 추가, 구조상의 유연성
- PROXY, 게이트웨이 같은 네트워크 기반의 중간매체 사용가능

## REST API 디자인
### 설계 시 가장 중요한 항목
1. URI는 정보의 자원을 표현해야 한다.
- 리소스명은 동사보다는 명사를 사용
- *틀린 예시)* `GET: /members/delete/1`
- Delete같은 행위에 대한 표현이 들어가는 안된다.
2. 자원에 대한 행위는 **HTTP Method(GET, POST, PUT, DELETE)**로 표현한다.
- 행위를 표현하는 예시) `DELETE: /members/1`
- 정보를 가져올 때는 GET, 추가시 POST  
`GET ) /members/show/1` **( X )**  
`GET ) /members/1` **( O )**

- `GET ) /members/insert/2` **( X )** : GET은 리소스 생성에 맞지 않는다  
- `POST ) /members/2` **( O )**

### HTTP Method의 알맞은 역할	

| Method | 역할 |
| :---: | :---: |
| **POST** | POST를 통해 해당 URI를 요청하면 리소스를 생성 |
| **GET**  | GET을 통해 해당 리소스 조회. 
| | 리소스를 조회하고 해당 문서에 대한 자세한 정보를 가져옴  |
| **PUT**  | PUT을 통해 해당 리소스를 수정함 |
| **DELETE** | DELETE를 통해 리소스를 삭제 |

- PUT을 대신 할 Method로 PATCH가 있다.
- PUT은 해당 자원의 전체를 교체한다는 의미, PATCH는 일부를 변경한다는 의미

### URI 설계 시 주의할 점
1. 슬래시 구분자( / ) 는 계층 관계를 나타냄
- Ex) `http://restapi.example.com/houses/apart`

2. URI 마지막 문자로 슬래시( / )를 포함하지 않는다
- URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 함
- URI가 다르다는 것은 리소스가 다르다는 것, 리소스가 다르다면 URI가 달라져야 함
- 슬래시를 사용해서 혼동을 주지 않도록 한다

3. 하이픈( - )은 가독성을 높이는데 사용
- URI가 불가피하게 길어지는 경우 하이픈을 사용

4. 밑줄( _ )은 URI에 사용하지 않는다
- 글꼴에 따라 다르긴 하지만 밑줄은 보기 어렵거나 문자를 가릴 수 있음
- 하이픈을 사용할 것

5. URI경로에는 소문자가 적합하다
- 대소문자에 따라 다른 리소스로 인식하게 됨

6. 파일 확장자는 URI에 포함시키지 않는다
- Ex) `http://restapi.example.com/members/soccer/123/photo.jpg` **( X )**
- REST API에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI에 포함시키지 않는다
- Accept header를 사용할 것
- `GET: /members/soccer/123/photo`  
`HTTP/1.1 Host: restapi.example.com`  
`Accept: image/jpg`

- Ex) `Accept: text/csv, text/html;q=0.5,application/xml;q=0.6`
- 0~1범위로 우선순위설정하여 응답가능
- 응답할 수 있는 media type이 Accept에 명시되지 않았다면 406응답과
Body에는 응답 가능한 media type을 명시하여 응답해야 함
	
### 리소스 간의 관계를 표현하는 방법
- REST리소스 간의 연관관계 표현
- Ex) `GET: /users/{userid}/devices`  
(일반적으로 소유 'has'의 관계를 표현할 때)
	
### 자원을 표현하는 Collection과 Document
- Document는 문서 / 한 객체로 취급
- Collection은 문서들의 집합 / 객체들의 집합으로 취급
- Ex) `http://restapi.example.com/sports/soccer/players/10`
- Sport, Players 컬렉션과 Soccer, 10번을 의미하는 Document로 이루어진 URI
- Collection은 복수로 사용

### 반응형 웹에서의 REST(플랫폼 중립적 URI)
- Ex) `http://m.restapi.com/123` 또는 `http://www.restapi.com/m/123` 처럼 사용 금지
- 기기 간 이동시 최적화 사이트 이동은 User-Agent header를 이용할 것
- 다국어 지원도 Accept-Language를 통해서 할것

### HTTP 응답 상태 코드
- 정확한 응답의 상태코드만으로도 많은 정보를 전달할 수가 있다

| Code | 내용 |
| :-------------: | :-------------: |
| **200** | 클라이언트의 요청을 정상적으로 수행 |
| **201** | 클라이언트가 리소스 생성을 요청, 해당 리소스가 성공적으로 생성 됨(POST를 통한 생성시) |
| **202** | 클라이언트의 요청이 비동기적으로 처리될 때 |
| | (응답바디에 처리되기까지의 시간 등의 정보) |
| **204** | 요청을 정상적으로 수행. 200과 다른점은 응답바디가 없을 때 |
| | (DELETE와 같은 요청 처리 후) |
| :-------------: | :-------------: |
| **400** | 클라이언트의 요청이 부적절 할 경우 |
| | (클라이언트에서 보낸 것들이 Validation을 통과하지 못했을 때. 응답바디에 실패이유 넣어줘야함) |
| **401** | 클라이언트가 인증되지 않은 상태에서 보호된 리소스를 요청했을 때 |
| | (로그인 하지 않은 유저가 로그인 했을때만 요청가능한 리소스를 요청했을 때) |
| **403** | 유저 인증상태와 관계없이 응답하고 싶지 않은 리소스를 클라이언트가 요청했을 때 |
| | (403 보다는 400이나 404를 사용할 것. 403자체가 리소스가 존재한다는 뜻 이기 때문에) |
| **405** | 클라이언트가 요청한 리소스에서는 사용 불가능한 Method를 이용했을 경우 |
| | (예: 읽기전용 리소스에 DELETE Method를 사용했을 때) |
| **408** | 요청 시간 초과 |
| **409** | 요청 처리 중 충돌발생(예 : 회원가입을 했는데 이미 사용하는 아이디) |
| :-------------: | :-------------: |
| **301** | 클라이언트가 요청한 리소스에 대한 URI가 변경되었을 때 |
| | (응답 시 Location header에 변경된 URI를 적어 줘야 함) |
| **302** | 임시로 주소가 바뀌었을 경우 |
| **304** | 이전에 방문했을 때의 요청과 다르지 않을 때(캐시페이지 사용) |
| **500** | 서버에 문제가 있을 경우 |
