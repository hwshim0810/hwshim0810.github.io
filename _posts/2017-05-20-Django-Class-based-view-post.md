---
layout: post
title: Django의 Class 기반 View 개념
description: "About class based view"
tags: [Django, Python]
---
# Class view의 시작
- URLconf 에서 함수형 뷰가 아닌 클래스 뷰를 사용하겠다고 선언
- URLconf는 요청과 관련된 Parameter를 클래스가 아닌 함수에 전달
	- 그래서 클래스 형 뷰는 클래스로 진입하기 위한 as_view()를 제공
		- 클래스 Method : 진입 Method

## as_view()의 역할
- 클래스의 인스턴스 생성
- 인스턴스의 dispatch() 호출
	- 요청을 검사하여 HTTP Method 판별
	- 인스턴스 내에서 해당 이름(HTTP Method)을 가지는 Method로 요청 중계
	- Method가 없다면 HttpResponseNotAllowed 예외발생

# Class view 의 내용
- 장고의 djnago.views.generic 의 제네릭 뷰를 상속받아 사용
	- .as_view()와 dispatch()를 사용할 수 있음

# Class view 의 장점
- HTTP Method를 if로 구분하지 않아 깔끔
- 다중상속 등의 객체지향적 코딩 가능, 제네릭 뷰, 믹스인 클래스 사용가능
	- 재사용성과 생산성 상승

# Generic view 의 종류
- Base view
	- 뷰 클래스를 생성, 다른 제네릭 뷰의 부모 클래스를 제공하는 기본 제네릭 뷰
	- View : 가장 기본이 되는 최상위 View
	- Templateview : 템플릿이 주어졌을 때 랜더링
	- Redirectview : url이 주어졌을 때 리다이렉트
- Generic display view
	- 객체의 리스트를 보여주거나, 특정 객체의 상태정보를 보여줌
	- Detailview : 객체하나에 대한 상세정보
	- Listview : 조건에 맞는 여러개의 객체를 보여줌
- Generic edit view
	- 폼을 통해 객체를 생성,수정,삭제하는 기능을 제공
	- Formview : 폼이 주어지면 해당 폼을 보여줌
	- Createview : 객체를 생성하는 폼
	- Updateview : 객체를 수정하는 폼
	- Deleteview : 객체를 삭제하는 폼
- Generic date view
	- 날짜 기반 객체의 연/월/일 페이지 구분해서 보여줌
	- Yeararchiveview : 년도가 주어지면 그에 해당하는 객체
- 등등...

# 예제
- Class 형 view로 처리
	
```python
# django import 생략…
 from .forms import MyForm

 class MyFormView(View):
  form_class = MyFrom # 보여줄 폼을 정의한 forms.py의 Class
  initial = {'key' : 'val'}
  template_name = 'form_template.html' # 폼을 포함하여 처리할 템플릿 이름

  def get(self, request, *args, **kwargs):
   form = self.form_class(initial=self.initial)
   return render(request, self.template, {'form' : form}

  def post(self, request, *args, **kwargs):
   form = self.form_class(request.POST)

   if form.is_valid():
    # cleaned_data 관련 로직…
    return HttpResponseRedirect('/success/') # 유효한 데이터를 처리

   return render(request, self.template_name, {'form' : form} # 유효하지 않은 데이터처리
```

- FormView 제네릭 뷰를 사용한 처리

```python
 from django.views.generic.edit import FormView
 from .forms import MyForm

 class MyFormView(FormView):
  form_class = MyForm
  template_name = 'form_template.html'
  success_url = '/complate/' # Class View 처리가 정상적으로 완료되었을 때 리다이렉트 시킬 URL

  # 유효한 폼 데이터로 처리할 로직. 반드시 super() 호출 필요
  def form_valid(self, form):
   # cleaned_data 처리 로직
   return super(MyFormView, self).form_valid(form)
```