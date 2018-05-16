---
layout: post
title: AWS Lambda API 사용시 POST/GET 데이터 변환문제
description: "AWS lambda traslate post data"
tags: [AWS, LAMBDA]
---

## 개요
- API Gateway 를 통한 AWS lambda 사용 시 JSON 형식이 아닌 요청 데이터의 경우 [예) `application/x-www-form-urlencoded`, QueryString] JSON 형식으로 변환해야 lambda 에서 event object 로 처리가능
- lambda 내부에서 변환이 가능하긴 하지만 템플릿을 이용해 내부작업을 줄이고 변환코드를 분리함

## 작업순서
- API Gateway 에 메서드를 만들고 lambda 와 연결했다고 가정함.

### POST
1. API Gateway 에서 메서드를 선택
2. 통합요청 선택
3. 본문 매핑 템플릿을 선택
4. '정의된 템플릿이 없는경우' 선택 -> Content type을 `application/x-www-form-urlencoded` 으로
5. 템플릿 생성 드롭다운에 '메서드 요청 패스스루' 선택 후 저장
6. API Gateway 배포

- 이전엔 템플릿 만들어진게 없었던 것으로 보이나 현재는 만들어진 '메서드 요청 패스스루'로 해결가능

### Get
- 위 4번의 Content type 을 `application/json` 으로
- 키가 'id' 인 쿼리스트링의 값을 'idx' 키로 수정해 전달함
```json
{ 
  "idx" : "$input.params('id')" 
}
```

## 완결
- 위의 방법을 사용하면 POST `application/x-www-form-urlencoded` 요청시 key-value가 아래와 같이 포함된다.
```json
{
...
"body-json": "id=user123",
...
}
```

- 아래 템플릿을 사용하면 lambda 내부 `event`를 통해 바로 key-value 사용가능
- 출처: https://forums.aws.amazon.com/thread.jspa?messageID=673012&tstart=0#673012

```text
## convert HTML POST data or HTTP GET query string to JSON
 
## get the raw post data from the AWS built-in variable and give it a nicer name
#if ($context.httpMethod == "POST")
 #set($rawAPIData = $input.path('$'))
#elseif ($context.httpMethod == "GET")
 #set($rawAPIData = $input.params().querystring)
 #set($rawAPIData = $rawAPIData.toString())
 #set($rawAPIDataLength = $rawAPIData.length() - 1)
 #set($rawAPIData = $rawAPIData.substring(1, $rawAPIDataLength))
 #set($rawAPIData = $rawAPIData.replace(", ", "&"))
#else
 #set($rawAPIData = "")
#end
 
## first we get the number of "&" in the string, this tells us if there is more than one key value pair
#set($countAmpersands = $rawAPIData.length() - $rawAPIData.replace("&", "").length())
 
## if there are no "&" at all then we have only one key value pair.
## we append an ampersand to the string so that we can tokenise it the same way as multiple kv pairs.
## the "empty" kv pair to the right of the ampersand will be ignored anyway.
#if ($countAmpersands == 0)
 #set($rawPostData = $rawAPIData + "&")
#end
 
## now we tokenise using the ampersand(s)
#set($tokenisedAmpersand = $rawAPIData.split("&"))
 
## we set up a variable to hold the valid key value pairs
#set($tokenisedEquals = [])
 
## now we set up a loop to find the valid key value pairs, which must contain only one "="
#foreach( $kvPair in $tokenisedAmpersand )
 #set($countEquals = $kvPair.length() - $kvPair.replace("=", "").length())
 #if ($countEquals == 1)
  #set($kvTokenised = $kvPair.split("="))
  #if ($kvTokenised[0].length() > 0)
   ## we found a valid key value pair. add it to the list.
   #set($devNull = $tokenisedEquals.add($kvPair))
  #end
 #end
#end
 
## next we set up our loop inside the output structure "{" and "}"
{
#foreach( $kvPair in $tokenisedEquals )
  ## finally we output the JSON for this pair and append a comma if this isn't the last pair
  #set($kvTokenised = $kvPair.split("="))
 "$util.urlDecode($kvTokenised[0])" : #if($kvTokenised[1].length() > 0)"$util.urlDecode($kvTokenised[1])"#{else}""#end#if( $foreach.hasNext ),#end
#end
}
```

- 참조 : [How to pass a params from POST to AWS Lambda from Amazon API Gateway](https://stackoverflow.com/questions/32057053/how-to-pass-a-params-from-post-to-aws-lambda-from-amazon-api-gateway/34844978?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)