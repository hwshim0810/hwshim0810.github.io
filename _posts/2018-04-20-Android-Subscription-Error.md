---
layout: post
title: 안드로이드 영수증처리 API 에러 처리
description: "Android validate api token error"
tags: [Android, ErrorHandling]
---

### Error
- 첫번째 에러
> 프로젝트 소유권 이전 후, 전에 쓰던 키가 이전의 프로젝트와 연결되어서 발생  

```json
{
  "code" : 403,
  "errors" : [ {
    "domain" : "androidpublisher",
    "message" : "The project id used to call the Google Play Developer API has not been linked in the Google Play Developer Console.",
    "reason" : "projectNotLinked"
  } ],
  "message" : "The project id used to call the Google Play Developer API has not been linked in the Google Play Developer Console."
}
```

### Handling
- Google API Console 에서 새로운 서비스계정과 비공개 키를 만들어서 인증파일을 교체함

### Error
- 두번째 에러
> Spring 서버에서는 .p12 키를 사용했는데, 이것은 인증시에 서비스계정 아이디를 필요로 하는데 이것을 교체하지 않았음.  

```json
{
  "error" : "invalid_grant",
  "error_description" : "Invalid JWT Signature."
}
```
```java
JsonFactory JSON_FACTORY = JacksonFactory.getDefaultInstance();
HttpTransport httpTransport = GoogleNetHttpTransport.newTrustedTransport();
GoogleCredential credential = new GoogleCredential.Builder()
  .setTransport(httpTransport)
  .setJsonFactory(JSON_FACTORY)
  .setServiceAccountId("...~~@~~.iam.gserviceaccount.com")  // 이곳에 들어가는 서비스계정 ID
  .setServiceAccountPrivateKeyFromP12File(new File("....~.p12"))
  .setServiceAccountScopes(Collections.singleton("https://www.googleapis.com/auth/androidpublisher"))
  .build();
```

### Handling
- 서비스 계정 ID를 교체하여 해결
