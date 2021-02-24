---
layout: post
title: "[Django] urls, render(template) 기능"
tags: [Web, Django]
use_math: true
comments: true
---

# Django

{% raw %}

- [urls.py](http://urls.py) 를 통해서 url 매핑

  config/[urls.py](http://urls.py) 에서는 include()를 통해 각 앱의 urls.py 들을 읽어들임.

  ```python
  from django.contrib import admin
  from django.urls import path, include

  urlpattrerns= [
  	path("", include("core.urls", namespace="core")),
  	path("admin/", admin.site.urls),
  ]
  ```

- templates를 사용하려면 [settings.py](http://settings.py) 의 TEMPLATES의 DIR에 경로를 추가해줘야 함. `os.path.join(BASE_DIR, "templates")`

- context - templates에 변수를 보내는 방법 중 하나

  ```sql
  return render(request, "html 파일명", context={})

  ```

- Template 확장 기능(상속이나 템플릿처럼 사용)
  - `{% extends "base.html" %}`
  - `{% block content %} {% endblock %}`

{% endraw %}
