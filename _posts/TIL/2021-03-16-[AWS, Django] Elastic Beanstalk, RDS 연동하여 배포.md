---
layout: post
title: "[AWS, Django] Elastic Beanstalk, RDS 연동하여 배포"
tags: [Web, Django, AWS, Deploy]
comments: true
---

# [AWS] Elastic Beanstalk, RDS, S3

### Elastic Beanstalk, RDS, Django 연동 (+S3)

<br>

**EB, RDS 연동 (보안 그룹 - 인바운드)**

RDS와 연동하는 과정에서 RDS 인스턴스의 보안 그룹에 Elastic Beanstalk 인스턴스의 보안그룹(sg-.....AWSEBSecurity .... 형태)을 추가해주었는데, 접근이라도 되는게 맞는데 계속 503 error가 발생했다.

이틀 정도를 고생하면서 여러 글들을 봤지만, 해결이 되지 않았다. 그 중 가장 상황에 맞는 해결책은 EB 인스턴스의 Configuration-인스턴스 설정-보안 그룹을 수정하는 것이었다. 인스턴스의 보안 그룹은 `sg-.....AWSEBSecurity ....` 로 되어있었다. RDS 보안 그룹은 `sg-21412qwe89e51 (default)` 이런식으로 다르게 되어있었기 때문에, RDS 인스턴스에 설정한 보안 그룹을 추가해줘야 할 것 같아서 설정을 바꿨다.

하지만.. 인스턴스가 설정을 바꾼다며 무한 로딩하다가 돌아오는건 `심각` 메시지와 이전 설정으로 되돌아간다는 `reverted` 메시지 뿐이었다.

계속 시도했지만 안되었고, 결국 인스턴스의 **환경 재구축**을 선택했다. 이것 마저도 에러를 뱉으며 안되길래, EB 인스턴스의 보안 그룹과 연결된 인바운드 규칙들을 모두 해제하고, 보안 그룹을 삭제한 후 환경 재구축을 재실행했더니 됐다.

그리고 보안 그룹을 추가하니 성공적으로 접속되었음.

<br>

**Error log (shell에서 `eb logs`)**

`eb deploy` 후 `eb logs` 로 에러를 확인하라고 배웠다. 하지만 deploy 후 log를 확인하면 실시간으로 업데이트가 되지 않았고, 표시 안되는 에러들도 많았다.

Elastic Beanstalk 콘솔을 여기저기 보다가 `로그` 페이지를 발견했다. 로그 페이지에서 '최근 100줄' 이 아닌 '전체' 를 선택하고 다운로드 받으면, 알집 파일로 로그들을 다운 받을 수 있다. 여기에는 정말 다 나와있는 것 같았다.

내가 겪은 경우에는 `cfn-init.txt, cfn-init-cmd.txt, eb-engine.txt, web.stdoput.txt` 만 확인했고, 거의 `cfn-init.txt, cfn-init-cmd.txt` 가 대부분이었다.

<br>

**EB 구성(Configuration) 활용**

그리고 EB의 소프트웨어 Configuration을 잘 활용하는게 중요하다. `SECRET_KEY, AWS_SECRET_ACCESS_KEY, RDS_PASSWORD` 등 많은 정보가 있는데, 이를 잘 활용하면 숨겨야 할 정보는 숨기면서 정보 보관도 깔끔하다.

<br>

---

settings.py 에서 선언해주어야 할 것들

```python
"""settings.py"""

if not DEBUG:
	DEFAULT_FILE_STORAGE = "config.custom_storages.UploadStorage"
	STATICFILES_STORAGE = "config.custom_storages.StaticStorage"
	AWS_ACCESS_KEY_ID = os.environ.get("AWS_ACCESS_KEY_ID")
	AWS_SECRET_ACCESS_KEY = os.environ.get("AWS_SECRET_ACCESS_KEY")
	os.environ["AWS_ACCESS_KEY_ID"] = AWS_ACCESS_KEY_ID
	os.environ["AWS_SECRET_ACCESS_KEY"] = AWS_SECRET_ACCESS_KEY
	AWS_STORAGE_BUCKET_NAME = "airbnb-clone-ting"
	AWS_DEFAULT_ACL = 'public-read'
	AWS_S3_CUSTOM_DOMAIN = f"{AWS_STORAGE_BUCKET_NAME}.s3.ap-northeast-2.amazonaws.com"
	STATIC_URL = f"https://{AWS_S3_CUSTOM_DOMAIN}/static/"
```
