---
layout: post
title: "[Django] Paginator (django.core.paginator)"
tags: [Web, Django]
use_math: true
comments: true
---

```python
from django.core.paginator import Paginator

paginator = Pagenator(`query_set`, `per_page`, `orphans`)
```

- 마지막 페이지에 오는 리스트들을 `orphans` 라고 하는데(`orphans_num` < `per_page`), `orphans` 를 설정해주면 마지막 페이지 이전 페이지에 `orphans`들을 포함시켜서 `per_page` 를 초과하여 출력해줌.

**Paginator.page(`page_num`=..)**

에러를 좀 더 컨트롤 할 수 있다.

**Paginator.get_page(`page_num`=..)**

에러 컨트롤이 적음. (쿼리스트링으로 `num_pages` 이상의 값을 넣어도 마지막 페이지 반환)

- Page.previous_page_number()
- Page.next_page_number()
  - 두 함수는 이전/다음 페이지가 없을 경우, InvalidPage 에러를 반환함.
