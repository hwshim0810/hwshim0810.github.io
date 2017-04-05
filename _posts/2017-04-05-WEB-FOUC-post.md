---
layout: post
title: FOUC(Flash of Unstyled Content)
description: "FOUC(Flash of Unstyled Content)"
tags: [Web, CSS, HTML]
---
## FOUC란?
- 브라우저로 웹문서에 접근했을 때 CSS가 모두 적용되지 못한 상태에서 화면이 표시되어 발생하는 화면 깜빡임, 스타일의 적용 전과 후가 그대로 화면에 노출된 상태로 변경되는 현상
- 해당 웹문서의 UX를 떨어뜨리는 요인으로 작용
- IE11에서도 발생되는 문제, 브라우저를 망라하고 목격되는 현상

## 발생원인 분석
- 브라우저의 동작방식과 연관이 있다  

1. 브라우저는 Markup에 참조된 모든 부수적인 파일들을 모아 즉시 DOM을 생성
2. 가장 빠르게 분석할 수 있는 글의 내용 부분을 화면에 표시
3. 화면에 표시된 내용을 선언된 마크업의 순서에 따라 스타일 적용하고 스크립트 실행  

- 여러개의 CSS파일 참조와 DOM구조 변경으로 인해 발생빈도 증가
- 웹 문서는 `@import`로 스타일파일을 가져오고, 다른 CSS를 참조할 수 있다
- 온라인 광고와 동영상과 검색 엔진같은 다른 곳에서 삽입된 콘텐츠는 종종 코드블록 내에서 자신의 스타일 규칙을 구술한다.
- **웹 문서가 모두 불린 후 스크립트를 이용해 DOM구조를 변경한다**
- 웹 문서들은 종종 프린터와 무선장치를 위한 CSS규칙으로 브라우저 화면 이외의 다른 **미디어에 대한 스타일 참조**를 포함한다
- **웹 폰트의 경우에도 IE에서는 FOUC를 유발한다**

## 해결 방법
- 기본적으로 `<head>`요소안에 CSS링크, `@import`의 사용을 자제
- 자바스크립트의 선언순서, 위치를 변경함으로써 극복 가능하거나, 매우 짧아질 수 있다.  
(`</body>`요소 앞에서 `<head>`안으로 위치를 변경해보는 방법)
- FOUC를 유발하는 구역을 숨겼다가 문서의 스타일이나 스크립트가 모두 적용되면 보여준다

- 예시  

```html
<html class="no-js">
    <head>
        <style>
            .js #fouc {display: none}
        </style> 
        <script>
            (function(H){H.className=H.className.replace(/\bno-js\b/,'js')})(document.documentElement)
        </script>
    </head>
    <body>
        <div id="fouc"> ... </div> <!-- /#fouc --> 
        <script> document.getElementById("fouc").style.display="block"; </script>
    </body>
</html>
```

`<html>`에 `.no-js`를 추가하고 `<head>`에 스타일과 자바스크립트를 추가한다.  
해당 자바스크립트는 브라우저의 자바스크립트가 활성화 되어 있을 경우,  
html 태그의 클래스를 `.js`로 변경한다. 그리고 숨겨질 구역에 ID 값으로 `id="fouc"`를 추가했다.  
위와 같이 페이지 전체를 감싸는 영역을 지정하면 빈 페이지를 보여주다가 모든 리소스가 로드되면 페이지를 보여주게 된다.
> IE에서는 웹폰트들에 대한 FOUC가 잦아 브라우저 스니핑으로 IE들을 골라내고 이에 대한 FOUC 처리를 하거나 숨겨질때 빈페이지를 보여주는 대신 로딩중이라는 표기나 이미지를 넣는 것도 방법이 될 수 있다.