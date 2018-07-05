---
layout: post
title: TypedArray 와 recycle
description: "Android TypedArray and recycle method"
tags: [Android, Java]
---

## 개요
- 안드로이드에서 CustomView 를 만들다보면 레이아웃의 속성을 만들어주고 설정해주는 코드가 나오게 된다.  
- 그 중 `TypedArray`를 가지고와서 쓴 후 보통 마지막에 `.recycle()`을 호출하고는 하는데, `TypedArray` 를 사용후에는 꼭 호출하라고 하는데,
이유를 명확히 설명해놓은게 없어서 직접 찾아보았다.

### 본문
- 호출의 목적은 Caching 때문인데, 내부적으로 `TypedArray` 는 Cache 를 위한 배열을 포함하고 있어 매번 할당하지 않기 위해 `Static field`로 Cache 된다.
- 아래의 코드를 참고해볼때, `recycle`을 호출하게되면 `Cache`로 사용한 객체를 반환해서 `Garbage Collection`의 대상에서 제외시키는 것으로 보인다.

```java
/**
 * Give back a previously retrieved StyledAttributes, for later re-use.
 */
public void recycle() {
    synchronized (mResources.mTmpValue) {
        TypedArray cached = mResources.mCachedStyledAttributes;
        if (cached == null || cached.mData.length < mData.length) {
            mXml = null;
            mResources.mCachedStyledAttributes = this;
        }
    }
}
```


### 나의생각
- CustomView가 거의 재사용되지 않는다면 굳이 호출하지 않아 GC 대상에 포함시키는게 맞는 것 같다.

### 참고
- [TypedArray.recycle](https://developer.android.com/reference/android/content/res/TypedArray#recycle%28%29)
- [StackOverflow](https://stackoverflow.com/questions/13805502/why-should-a-typedarray-be-recycled)