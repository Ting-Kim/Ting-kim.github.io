## 4th week - 라이브 세션

`**get_json()` 메서드의 정체 → 딕셔너리 형태로 access\*\*

- HTTP 프로토콜 상에서 바디에 담겨있는 JSON은 "문자열" 취급된다.
- JSON 형태의 문자열 → 파이썬의 딕셔너리 변환

**flask의 app 이름은 반드시 `app.py` 여야 하는가?**

- `flask run` 명령을 실행하면 FLASK_APP 변수에 담긴 파일명을 실행
- 만약 해당 파일이 없다면 `[app.py](http://app.py)` 를 default로 찾고, 그것도 없으면 에러 발생

---

## Django 의 MVT 패턴

- Model, View, Template 으로 구성
- 일반적인 MVC 패턴에서의 Controller 역할을 urls.py가 수행하고,

<br>

**MTV 구성**

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/MTVPattern.png?raw=true](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/MTVPattern.png?raw=true)
(ref:https://www.google.com/url?sa=i&url=https%3A%2F%2Fcupjoo.tistory.com%2F91&psig=AOvVaw37EW2X3I9_9Ms4ZLpRSi05&ust=1608899634903000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCLC-sq3Q5u0CFQAAAAAdAAAAABAJ)

<br>

### View

```python
<!--
- <project>/urls.py 에서 url 매핑
- <project>/settings.py - INSTALLED_APPS 에 App을 추가해주어야 project에서 App으로 인식
- <App name>/views.py 에서 요청에 대한 로직을 수행
- return render(request, <template 파일명>, {<Template에 전달할 key>:<value> })
-->
<!-- Template 태그는 -->
'{% tag %} ... {% end tag %}' 형태로 사용
```

<br>

### Template

```python
- Template는 <project>/settings.py 의 TEMPLATES 에서 관리
- Template 변수는 html에서는 '{{<변수명>}}' 으로 사용
```
