---
layout: post
title: Django Admin 성능 최적화
description: "Accelerate Django Admin"
tags: [Django, Python]
---

### 개요
- Django Admin 을 이용한 작업 중 특정 모델의 관리 페이지가 느려지는 현상을 발견함

### 원인
- 데이터가 100만건이 넘어가니 문제가 눈에 띄게 발생함

- Django debug toolbar 를 이용하여 SQL을 조회하니 비용이 큰 SQL 문이 조회됨

```sql
# 심지어 두번 조회중!
SELECT COUNT(*) AS "__count" FROM "table_name"
```

### 해결
1.  관리자 페이지 상단에 전체건수를 적어주기 위해 1번 조회가 됨

```python
# Admin 클래스 내부에 속성을 추가함
class SomeAdmin(admin.ModelAdmin):
    show_full_result_count = False
```

2. 그래도 1번이 남았는데 이것은 Django Paginator 에 의해서 발생함
- Count 근사치를 캐시해조는 방법으로 해결 가능
- 페이지 넘버가 커지는 경우 꼬일 가능성 있음
- 초기 로드시 Count 조회 후 이후 캐시값 사용
- 아래의 캐시된 Paginator Class 를 Admin class 에서 사용
```python
class SomeAdmin(admin.ModelAdmin):
    paginator = CachingPaginator
```

- 일반적 해결
- MySQL에서 잘됨. 다른 DB도 동작할것으로 예상
```python
class CachingPaginator(Paginator):
    def _get_count(self):

        if not hasattr(self, "_count"):
            self._count = None

        if self._count is None:
            try:
                key = "adm:{0}:count".format(hash(self.object_list.query.__str__()))
                self._count = cache.get(key, -1)
                if self._count == -1:
                    self._count = super().count
                    cache.set(key, self._count, 3600)

            except (AttributeError, TypeError):
                # AttributeError if object_list has no count() method.
                # TypeError if object_list.count() requires arguments
                # (i.e. is of type list).
                self._count = len(self.object_list)
        return self._count

    count = property(_get_count)
```

- MySQL 사용가능
```python
class ApproxCountQuerySet(QuerySet):
    def count(self):
        if self._result_cache is not None and not self._iter:
            return len(self._result_cache)

        is_mysql = 'mysql' in connections[self.db].client.executable_name.lower()

        query = self.query
        if (is_mysql and not query.where and
                query.high_mark is None and
                query.low_mark == 0 and
                not query.select and
                not query.group_by and
                not query.having and
                not query.distinct):
            # If query has no constraints, we would be simply doing
            # "SELECT COUNT(*) FROM foo". Monkey patch so the we
            # get an approximation instead.
            cursor = connections[self.db].cursor()
            cursor.execute("SHOW TABLE STATUS LIKE %s",
                    (self.model._meta.db_table,))
            return cursor.fetchall()[0][4]
        else:
            return self.query.get_count(using=self.db)
```

### 추가
- 외래키 속성 조회가 잦은 경우 select_related 이용
- 단, 데이터 건수가 많은 경우 Join 이 시간을 너무 소모하게 됨

```python
# Admin class 에서 get_queryset Method Override
class SomeAdmin(admin.ModelAdmin):
    # ...

    def get_queryset(self, request):
        qs = super(SomeAdmin, self).get_queryset(request)
        return qs.select_related('some_foreign_field')

    # ...
```

- [Reference](https://stackoverflow.com/questions/10433173/prevent-django-admin-from-running-select-count-on-the-list-form)