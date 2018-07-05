---
layout: post
title: 안드로이드 ListView 스크롤 중간 멈춤 문제
description: "Android ListView Scroll error"
tags: [Android, ErrorHandling]
---

### Error
- 리스트뷰 초기 로드 시 스크롤이 중간에 멈추는 현상

```java
listView.post(() -> listView.smoothScrollToPosition(adapter.getCount()-1));
```

- 위와 같은 형태에서 어뎁터의 데이터 추가 문제 때문인지 스크롤이 중간에 자꾸 멈춤

### 시도
1. `android:transcriptMode` 속성 이용
- Listview 속성으로 아이템 추가시 Focus 이동의 형태를 결정함
  - disabled
    - 기본속성: 추가되더라도 아무 반응 없음
  - normal
    - 아이템이 추가되었을 때, 마지막 아이템에 Focus 되어 있었다면 새로 추가된 쪽으로 Focus 이동
  - alwaysScroll
    - 아이템이 추가되었을 때, 무조건 마지막으로 이동

- 저것만으로는 초기 로드시 문제가 해결이 되지는 않았음

2. `android:stackFromBottom` 속성
- true / false
- Listview 속성으로 아이템이 쌓이는 방향을 결정
- true 값을 주고 위 속성과 같이 쓰니 해결은 되었으나 아이템 갯수가 적어 스크롤이 없는 경우에는 맨 아래부터 아이템이 추가되어서 그닥 보기가 좋지않음

3. `onScrollStateChanged` 이벤트
- 이전 스크롤 이동 Method 에서 공유가능한 상태값을 설정해 스크롤 상태변화 이벤트에서 마무리

```java
public void onScrollStateChanged(AbsListView view, int scrollState) {
    if (lastLoadState && view.canScrollVertically(-1)) {
      lastLoadState = false;
      listView.setSelection(adapter.getCount()-1);
    }
  }
```

- `canScrollVertically(-1)` 이 true 인 경우는 아래 스크롤이 남았다는 것. 끝이라면 false
- `canScrollVertically(1)` 은 위쪽 스크롤 상태 체크

### 후기
- 데이터 로드의 개선으로 해결가능할 것으로 보이나 애매한 경우 이렇게 해결가능한 듯