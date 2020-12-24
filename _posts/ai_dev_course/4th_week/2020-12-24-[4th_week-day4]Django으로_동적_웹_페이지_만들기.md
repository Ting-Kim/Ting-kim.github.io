**`models.py` : DB 테이블과 매칭되는 객체를 선언(ORM 방식)**

- model class들을 만들어놓은 상태에서 `python manage.py makemigrations <app_name>` (마치 git add)을 해줘야 장고에서 class들을 migration으로 등록해준다.
- 이후 `python manage.py migrate`(마치 commit, push)를 하여 실제 DB에 반영하는 작업을 진행한다.

```python
from django.db import models

# Create your models here.
class Coffee(models.Model):
    def __str__(self):
        return self.name
    """
    문자열 : CharField
    숫자 : IntegerField, SmallIntegerField, ..
    논리형 : BooleanField
    시간/날짜 : DateTimeField
    """
    # Field 1
    name = models.CharField(default="", max_length=30)
    # Field 2
    price = models.IntegerField(default=0)
    # Field 3
    is_ice = models.BooleanField(default=False)
```

<br>

**`views.py` : request를 처리하는 로직 구현**

- **template**을 랜더링 할 수 있다.
- dictionary 형태로 **template**에 파라미터를 전달할 수 있다.

```python
from django.shortcuts import HttpResponse, render
from .models import Coffee
from .forms import CoffeeForm

# Create your views here.
def index(request):
    # return HttpResponse("<H1>Hello World!<H1>")
    number = 10
    name = "micheal"
    nums = [1,2,3,4,5]
    return render(request, 'index.html', {"my_num":number, "my_name":name, "my_list":nums})

def coffee_view(request):
    coffee_all = Coffee.objects.all()
    # 만약 reqeust가 POST라면:
    if request.method == "POST":
        # POST를 바탕으로 Form을 완성하고
        form = CoffeeForm(request.POST)
        # Form이 유효하면 -> 저장
        if form.is_valid():
            form.save()

    form = CoffeeForm()
    return render(request, 'coffee.html', {"coffee_list":coffee_all, "coffee_form":form})
```

<br>

**`forms.py` : 클라이언트 측에서 입력할 수 있는 Form을 구현하는 클래스**

- 내부 클래스인 Meta를 선언하여 어떤 모델에 대한 form인지 정의할 수 있다.
  (해당 클래스에 맞는 form 생성)

```python
from django import forms
from .models import Coffee # Model 호출

class CoffeeForm(forms.ModelForm): # ModelForm을 상속받는 CoffeeForm 생성
    class Meta: # form을 만들기 위해서 어떤 클래스가 필요한지
        model = Coffee
        fields = ("name", "price", "is_ice")
```

<br>

`template/coffee.html`

- `<form_name>.as_p` : `forms.py`의 form 입력을 위한 html 코드를 자동 입력 가능함.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Coffee List</title>
  </head>

  <body>
    <h1>welcome to Ting's Coffee</h1>
    <p>메뉴 : {{coffee_list}}</p>
    {% raw %} {% for coffee in coffee_list %} {% endraw %}
    <hr />
    <p>이름 : {{coffee.name}}</p>
    <p>가격 : {{coffee.price}}</p>
    <hr />
    {% raw %}{% endfor %}{% endraw %}

    <form method="POST">
      {% raw %} {% csrf_token %} {{coffee_form.as_p}} {% endraw %}
      <button type="submit">Save</button>
    </form>
  </body>
</html>
```
