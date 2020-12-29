---
layout: post
title: "[python_base64]How to use base64 Module"
tags: [python, base64, encoding]
use_math: true
comments: true
---

## python - base64 모듈 사용법

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/20201229_3.PNG?raw=true](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/20201229_3.PNG?raw=true)

- `b = s.encode("UTF-8")`

  문자열 → 바이트 변환

- `e = base64.b64encode(b)`

  바이트 → base64 바이트 변환

- `s1 = e.decode("UTF-8")`

  base64 바이트 → base64 문자열 변환

- `b1 = s1.encode("UTF-8")`

  base64 문자열 → base64 바이트 변환

- `d = base64.b64decode(b1)`

  base64 바이트 → 바이트 변환

- `m = base64.b64decode(s)`

  바이트가 아닌 문자열을 base64 인코딩 실행하여 에러 발생

  **에러 문구 :** **binascii.Error: Incorrect padding**

<br>

### 이미지를 base64를 이용하여 인코딩하기

**syntax of the data URI scheme : `data:[<media type>][;charset=<character set>][;base64],<data>`**

사용 예 )

```html
<div>
  <p>Taken from wikpedia</p>
  <img
    src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAAUA
    AAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO
        9TXL0Y4OHwAAAABJRU5ErkJggg=="
    alt="Red dot"
  />
</div>
```

```python
# views.py #

# Create your views here.
def myeda(request):
    head = insurance_df.head(5).to_html()
    tail = insurance_df.tail(5).to_html

    sns.stripplot(x="region", y="children", data=insurance_df)
    my_stringIOBytes = BytesIO()
    plt.savefig(my_stringIOBytes, format='png')
    my_stringIOBytes.seek(0)
    my_base64_pngData = base64.b64encode(my_stringIOBytes.read()).decode("UTF-8")

    html = '<h3>header check</h3>'
						+'<img src=\"data:image/png;base64,{}\">'.format(my_base64_pngData)
						+ '<h3>tail check</h3>'

    context = {'head':head, 'tail':tail, 'graph1':html, 'test':'test'}
    return render(request, 'myeda.html', context)
```

```html
<!-- myeda.html -->

<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>EDA project</title>
  </head>
  <body>
    <p>Hello World</p>
    {% raw %} {% autoescape off %}
    <p>head(5)</p>
    {{ head }}

    <p>tail(5)</p>
    {{ tail }} {{ graph1 }} {% endautoescape %} {% endraw %}
  </body>
</html>
```

**결과 화면 `Complete!`**

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/20201229_4.PNG?raw=true](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/20201229_4.PNG?raw=true)

---

reference)

- Base64 인코딩 및 디코딩 - [https://riptutorial.com/ko/python/example/27070/base64-인코딩-및-디코딩](https://riptutorial.com/ko/python/example/27070/base64-%EC%9D%B8%EC%BD%94%EB%94%A9-%EB%B0%8F-%EB%94%94%EC%BD%94%EB%94%A9)
- matplotlib embed figures in auto generated html [duplicate] (StackOverFlow) - [https://stackoverflow.com/questions/48717794/matplotlib-embed-figures-in-auto-generated-html](https://stackoverflow.com/questions/48717794/matplotlib-embed-figures-in-auto-generated-html+)
- How to display Base64 images in HTML? (StackOverFlow) - [https://stackoverflow.com/questions/8499633/how-to-display-base64-images-in-html](https://stackoverflow.com/questions/8499633/how-to-display-base64-images-in-html+)
- **Matplotlib graphic image to base64 (StackOverFlow)** - [https://stackoverflow.com/questions/38061267/matplotlib-graphic-image-to-base64](https://stackoverflow.com/questions/38061267/matplotlib-graphic-image-to-base64)
