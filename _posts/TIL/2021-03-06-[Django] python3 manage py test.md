---
layout: post
title: "[Django] python3 manage py test"
tags: [Web, Django, TDD]
comments: true
---

- `INSTALL_APPS` 에 등록을 하지 않은 APP에서의 test 파일들도 실행시킨다.
- 'test' 로 시작하는 파일들을 test파일이라 간주하고 실행시킨다.

`atests_all.py` 파일은 실행시키지 않지만, `tests_all.py` 파일은 실행시킨다.

<br>

모듈 설치

- `pip3 install pytest`
- `pip3 install pytest-django`
- `pip3 install pytest-cov`
- `pip3 instasll ipdb` (테스트 코드에 브레이킹 시켜서 디버깅 기능 제공)

<br>

```python
class PostsBaseTest(TestCase):
	# model test
	def test_create_user_model(self):
	      User.objects.create(
	          username='Hello_World'
	      )
				import ipdb;ipdb.set_trace() # ipdb 사용 예
	      assert User.objects.count() == 1, "Should be equal"  # user 테이블에서 긁어온 user 객체 수
```

<br>

---

**+@**

- how to calculate coverage 한번 검색해서 보기
