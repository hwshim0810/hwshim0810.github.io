---
layout: post
title: AMP(Accelarated Mobile Page)
description: "AMP(Accelarated Mobile Page)"
tags: [Web, AMP, Mobile]
---
- 모바일 웹에서 광고와 같은 부수적 콘텐츠로 인해 페이지 로딩이 느린 문제 개선
- 페이지 로딩 완료전까지 스크롤이 되지 않는 문제 개선
- 모바일 기기에서 웹 사이트의 접근성을 높이기 위한 가속화 모바일 페이지
- 즉시에 가까운 페이지 로딩을 위해 여러 기법으로 최적화 하였고  
**AMP HTML** / **AMP JS** / **Google AMP Cache**로 구성

***

## 동작 원리
#### 1. AMP에 포함되는 모든 JS는 비동기 방식으로 실행된다
- AMP에는 개발자가 직접 작성한 별도의 JS를 포함할 수 없다
- 페이지 동작과 관련된 부분은 커스텀 AMP요소를 통해 구현할 수 있다
- Iframe안에 별도로 외부 JS라이브러리를 포함할 수 있다  
(메인페이지 렌더링에 방해 안 됨)


#### 2. 이미지, 광고, iframe같은 외부 리소스들의 사이즈와 위치 지정필요
- 외부 리소스를 다운받기 전에 HTML요소의 사이즈를 지정하면, 다운로드 여부와 관계없이	페이지 레이아웃을 정할 수 있다
- 이러한 원리로 전체 문서의 레이아웃을 정하기 위해 단 한번의 요청만 필요
- 리소스가 로딩 될 때 레이아웃을 다시 그릴 필요가 없어짐


#### 3.커스텀 스크립트를 사용하는 경우 커스텀 태그를 지정해주어야 한다
```html
<script async custom-element="amp-iframe" src="https://cdn.ampproject.org/v0/amp-youtube-0.1.js"></script>
```


#### 4. 외부 라이브러리들이 렌더링 동작에 방해되지 않도록 한다
- AMP는 외부 자바스크립트 라이브러리를 iframes안에서만 허용한다  
(메인페이지 실행방해 X)
- Iframes에서 스타일 재계산이나 페이지 레이아웃 재조정이 일어나더라도, DOM 사이즈가 작기 때문에 속도가 빠르다


#### 5. 모든 CSS스타일은 인라인이어야하고 사이즈가 제한되어 있다
- 인라인 CSS스타일은 1개만 허용되기 때문에 다른 웹 사이트에 비해 최소 1개 이상의 HTTP요청을 줄인다
- 인라인 CSS스타일의 최대사이즈는 50kb
	
#### 6. 웹 폰트 요청은 효율적으로 해야 한다
- 일반적인 웹 페이지에서는 외부 JS와 스타일 시트를 순차적으로 로딩 후 폰트다운
- AMP에서는 폰트를 받기까지 외부JS와 스타일 시트를 다운받는 HTTP요청이 존재하지 않음
- JS의 경우 async속성, 스타일 시트의 경우 인라인을 이용하기 때문


#### 7. 스타일 재계산 최소화
- 페이지에서 요소 측정이 일어날 때마다 스타일이 쟤 계산
- 이는 전체 페이지 레이아웃 재조정과 연결되기 때문에 많은 비용이 소모
- 모든 DOM을 화면에 그리기 전에 읽기 때문에 한 프레임당 최대 한번만 스타일을 재계산


#### 8. GPU가속화 애니메이션만 사용
- 빠른 성능을 위한 최적화 방법은 GPU위에서 애니메이션을 실행하는 것
- GPU의 단점은 페이지 레이아웃 재조정을 못하는 것인데 이것을 보통 브라우저에 위임
- 애니메이션은 GPU에서 실행될 수 있도록 CSS규칙을 정해야 한다
- 특히 AMP는 애니메이션 작업과 변환 작업을 transform과 opacity에서 하기 때문에 레이아웃 조정이 필요없다


#### 9. AMP는 리소스 로딩 순서를 조정한다
- AMP는 리소스 다운로드를 모두 제어하고 중요도에 따라 순서를 정한다
- 이미지나 광고는 필요한 경우 (스크롤해서 보여지는 경우) 에만 재빨리 다운로드 한다
- Lazyloading 관련 리소스는 pre-fetch를 해놓기 떄문에, 로딩이 되어야하는 시점에 더 빠르게 로딩되고 CPU는 필요시에만 사용된다


#### 10. 페이지 즉시 로딩하기
- DNS Lookip, TCP Handshake등을 미리 처리하는 preconnect API로 요청을 최대한 빨리한다
- Prerending의 경우 모든 웹 컨텐츠에 적용되며 많은 양의 대역폭과 CPU를 소비한다
- AMP는 위의 단점들을 보완하여 최적화, CPU가 많이 소비되는 rendering의 경우에는 prerendering을 하지 않는다

## AMP 코드 예시
- AMP Html Example

```html
<!doctype html>
<html amp lang="en">
  <head>
    <meta charset="utf-8">
    
    <!-- AMP 자바스크립트 라이브러리 로딩 --> 
    <script async src="https://cdn.ampproject.org/v0.js"></script>
    
    <title>Hello, AMPs</title>
    <link rel="canonical" href="http://example.ampproject.org/article-metadata.html" />
    <meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
    <script type="application/ld+json">
      {
        "@context": "http://schema.org",
        "@type": "NewsArticle",
        "headline": "Open-source framework for publishing content",
        "datePublished": "2015-10-07T12:02:41Z",
        "image": [
          "logo.jpg"
        ]
      }
    </script>
    
    <!-- AMP CSS 스타일 -->
    <style amp-boilerplate>
        body{-webkit-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-moz-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-ms-animation:-amp-start 8s steps(1,end) 0s 1 normal both;animation:-amp-start 8s steps(1,end) 0s 1 normal both}
        @-webkit-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}
        @-moz-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}
        @-ms-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}
        @-o-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}
        @keyframes -amp-start{from{visibility:hidden}to{visibility:visible}
    </style>
    <noscript>
        <style amp-boilerplate>
            body{-webkit-animation:none;-moz-animation:none;-ms-animation:none;animation:none}
        </style>
    </noscript>
  </head>
  <body>
    <h1>Welcome to the mobile web</h1>
  </body>
</html>
```

- AMP 비디오 추가

```html
<amp-video width=400 height=300 src="https://yourhost.com/videos/myvideo.mp4" poster="myvideo-poster.jpg">
  <div fallback>
    <p>Your browser doesn’t support HTML5 video</p>
  </div>
  <source type="video/mp4" src="foo.mp4">
  <source type="video/webm" src="foo.webm">
</amp-video>
```

- AMP 이미지 추가

```html
<amp-img src="welcome.jpg" alt="Welcome" height="400" width="800"></amp-img>
```

- AMP에서 일반 Html요소는 그대로 사용, img는 amp-img로 사용
- 몇 개의 Html표준 태그는 AMP에서 사용불가(embed, param등) [참고](https://github.com/ampproject/amphtml/blob/master/spec/amp-html-format.md)
- 따로 img태그를 사용하는 이유는 해당 리소스가 로딩되기 전에 페이지 레이아웃을 정하고 Lazyloading에 대한 네트워크 제어와 리소스 로딩 우선순위의 효율적 관리를 위함

- AMP CSS 추가  
    a. `<style amp-custom>`을 이용하여 요소 스타일링
    
```html
<style amp-custom>
  /* any custom style goes here */
  body {
    background-color: white;
  }
  amp-img {
    background-color: gray;
    border: 1px solid black;
  }
</style>
```

### 주의할 점
- 모든 AMP페이지는 `<style amp-cunstom>` 태그 한 개만 포함할 수 있다
- Html인라인 CSS를 사용할 수 없다. 모든 스타일 규칙은 `<head>`안에 선언되어야 함
- `!important`를 비롯한 몇몇 표준 스타일 규칙 사용불가, 외부 스타일시트 참조 불가(커스텀 폰트 제외)

***

## 유효성 검사
• 해당 문서의 AMP규칙 준수 여부는 URL끝에 **#development=1**을 추가하여 개발자도구 콘솔에서 검사가능

***

## AMP 검색과 배포
- 같은 웹 콘텐츠에 대해 AMP, non AMP페이지 2개를 모두 갖고 있는 경우  
`<link>`를 이용하여 두 페이지를 연결한다

- 일반 HTML페이지

```html
<link rel="amphtml" href="https://www.example.com/url/to/amp/document.html">
```
    
- AMP 페이지
    
```html
<link rel="canonical" href="https://www.example.com/url/to/full/document.html">
```

- 구분없이 AMP만 있는 경우
    
```html
<link rel="canonical" href="https://www.example.com/url/to/amp/document.html">
```