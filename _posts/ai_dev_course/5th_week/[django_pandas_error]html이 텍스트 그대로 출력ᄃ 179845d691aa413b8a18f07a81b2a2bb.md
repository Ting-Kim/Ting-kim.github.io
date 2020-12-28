# [django_pandas_error]html이 텍스트 그대로 출력되는 문제

## 문제점

`pandas` 이용해서 데이터 `csv 파일`을 불러오고, html에 표현하기 위해 dataframe 형태에 `.to_html()` 을 사용해서 html 코드를 head 변수에 담아서 템플릿에 변수로 넘겨줬다.

```python
# views.py #

from django.shortcuts import render
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from pathlib import Path
import os

BASE_DIR = Path(__file__).resolve().parent.parent
insurance_df = pd.read_csv(os.path.join(BASE_DIR, 'eda','static', 'eda','insurance.csv'))

# Create your views here.
def myeda(request):
    head = insurance_df.head(5).to_html()
    tail = insurance_df.tail(5)
    print(head)

    context = {'head':head, 'tail':tail, 'test':'test'}
    # context = {}

    return render(request, 'myeda.html', context)
```

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>EDA project</title>
  </head>
  <body>
    <p>Hello World</p>
    {{ head }}
  </body>
</html>
```

그런데.. html 태그들이 반영이 안되고 그대로 출력이 되었다.

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/20201229_1.png?raw=true](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/20201229_1.png?raw=true)

---

## 해결방안

알아보니 Django에서는 기본적으로 `esacape` 기능이 활성화 되어있다고 한다. 그래서 이 기능을 `{% autoescape off %}` 해주면 정상적으로 출력된다!

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>EDA project</title>
  </head>
  <body>
    <p>Hello World</p>
    {% autoescape off %} {{ head }} {% endautoescape %}
  </body>
</html>
```

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/20201229_2.png?raw=true](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/20201229_2.png?raw=true)

---

ref)

- 장고(django)에서 넘겨받은 값에 html이 그대로 보인다면 - [https://devlog.jwgo.kr/2018/03/20/django-escape-issue/](https://devlog.jwgo.kr/2018/03/20/django-escape-issue/)
