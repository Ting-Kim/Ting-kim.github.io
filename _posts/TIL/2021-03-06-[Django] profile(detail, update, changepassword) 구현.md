---
layout: post
title: "[Django] profile(detail, update, changepassword) 구현"
tags: [Web, Django]
use_math: true
comments: true
---

{% raw %}

### user profile page 구현

django.views.generic.DetailView

제네릭 뷰(DetailView)를 통해 구현.

사용한 속성 - `model`, `template_name`, `context_object_name`

context_object_name의 default는 'object'인데, template 에서 해당 객체를 호출할 때 쓸 변수명이다. {{context_object_name(`default='object'`)}}

- 로그인해서 접속해 있는 유저와 보고있는 프로필 페이지 주인인 유저를 비교할 때 사용함 ({% if user ==  user_obj %})

<br>

### update user profile page 구현

제네릭 뷰(UpdateView)를 통해 구현.

사용한 속성 - `model`, `template_name`, `fields`

- `fields` 중 "fav_book_cat", "fav_movie_cat" 는 `categories.Category`와 외래키로 관계된 컬럼인데, profile을 update할 때 꼭 설정하지 않으면 수정이 불가했다. 그래서 models.py에서 `blank=True`를 줘서 일단 해결함.
- `urls.py` 에서 profile 페이지에서는 url에 &lt;int:pk&gt;를 포함하였는데, update 시킬 땐 url에서 제외하니까, UpdateView가 default로 url에서 pk를 찾기 때문에 에러가 발생했다. 그래서 get_object() 메서드를 선언하여 self.request.user 를 반환하게 함.

<br>

### change user password page 구현

제네릭 뷰(PasswordChangeView)를 통해 구현.

사용한 속성 - `template_name`, `success_url`

알아서 password Form을 만들어주고, POST 로 요청 보내면 수정까지 해준다.

success_url에 reverse("core:home")을 주었더니, 아래 에러가 발생함.

```python
""" success_url에 reverse() 사용 시 에러 """
django.core.exceptions.ImproperlyConfigured: The included URLconf 'config.urls' does not appear to have any patterns in it. If you see valid patterns in the file then the issue is probably caused by a circular import.
```

알아보니 reverse 메서드를 import 하는 시점에 reverse() 가 호출되어 버리기 때문이라고 한다.(urls.py가 로드되기 전에)

reverse_lazy() 의 경우에는 직접 실행 될 때 불러오기 때문에 정상적으로 실행이 됨. (Django 공식 문서에서도 CBV(제네릭 뷰) 사용 시 reverse_lazy() 를 사용해야 한다고 명시되어 있다고 한다.)

{% endraw %}
