---
layout: post
title: "[5th_week_proj]Django deploy using uWSGI, Nginx at Ubuntu"
tags: [Django, uWSGI, Nginx]
use_math: true
comments: true
---

# Ubuntu 환경에서 uWSGI, Nginx를 사용해서 Django 프로젝트 배포하기

**Wsgi**, 웹 서버 게이트웨이 인터페이스) (파이썬에서는 Uwsgi라고 함)

- 소켓이 자동 생성되는데, 요청이 들어왔을 때 자동으로 연결해주는 역할(?)
- Nginx 와 연결다리 역할(?)

이 [유튜브 영상](https://www.youtube.com/watch?v=oGQ1HteFYnQ)을 참고하였다가 미친듯한 에러들을 겪었다.

**에러문구**

`2020/12/31 18:15:12 [error] 5214#5214: *1 upstream prematurely closed connection while reading response header from upstream, client: 221.151.46.236, server: _, request: "GET /favicon.ico HTTP/1.1", upstream: "uwsgi://unix:/home/ubuntu/django-eda/myeda/uwsgi.sock:", host: "52.78.60.138", referrer: "[http://52.78.60.138/](http://52.78.60.138/)" "error.log" [readonly] 42L, 11592C`

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/20210103_2.PNG?raw=true](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/20210103_2.PNG?raw=true)

이건 어떻게 해결했는지 기억도 안 나네.. 😱

<br>

---

## 새로운 마음으로 **처음부터 다시 시도..!**

느낌이 좋은 곳을 찾아서 이번엔 이 [포스팅](https://yuddomack.tistory.com/entry/처음부터-시작하는-EC2-python-설치?category=777812)을 참고하였다.

<br>

**겪었던 에러**

- `uwsgi: error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such file or directory` - [https://stackoverflow.com/questions/43301339/pcre-issue-when-setting-up-wsgi-application/50087846](https://stackoverflow.com/questions/43301339/pcre-issue-when-setting-up-wsgi-application/50087846) , [https://github.com/unbit/uwsgi/issues/1770](https://github.com/unbit/uwsgi/issues/1770)
- `!!! Python Home is not a directory: ../myvenv/ !!! Set PythonHome to ../myvenv/ Fatal Python error: initfsencoding: Unable to get the locale encoding ModuleNotFoundError: No module named 'encodings' Current thread 0x00007f2a61551740 (most recent call first): Aborted (core dumped)` - 가상환경 경로 설정 문제였다 ([https://stackoverflow.com/questions/60809917/django-w-uwsgi-fatal-python-error-initfsencoding-unable-to-get-the-locale-enc](https://stackoverflow.com/questions/60809917/django-w-uwsgi-fatal-python-error-initfsencoding-unable-to-get-the-locale-enc))

<br>

uWSGI **공식 문서**

- [https://uwsgi-docs.readthedocs.io/en/latest/WSGIquickstart.html](https://uwsgi-docs.readthedocs.io/en/latest/WSGIquickstart.html)

<br>

### 에러 해결 내용(링크 포함)

- [근본적인 python, pip 버전에 대한 지적 ('uWSGI runs wrong version of Python' - StackOverFlow)](https://stackoverflow.com/questions/20531131/uwsgi-runs-wrong-version-of-python)

  ⇒ 대부분의 패키지들이 pip를 통해 설치되는데, pip는 환경에 default로 설정된 python을 기반으로 탑재 되어있다. uwsgi도 pip를 통해 설치하기 때문에, uwsgi는 설치될 때의 pip에 해당하는 python 버전에 의존한다. 그래서 우분투 환경에서 uwsgi를 비롯한 패키지를 사용할 때는 **초기 접속하자마자 python, pip 등의 근본적인 버전 설정(default 포함)하는 것**이 중요하다. AWS_EC2의 경우 기본적으로 `/home/ubuntu/anaconda3/bin/python` 에 디폴트로 설정되어 있으므로 이걸 `/usr/bin/local/python` 로 설정을 변경하는게 중요하다. 변경하는 방법은 [여기](https://www.notion.so/AWS-EC2-python-pip-default-80be1f872e544d91b0d2ca383d74e714)를 참고하자.

- `"Could not install packages due to an EnvironmentError: [Errno 13] Permission denied: '/usr/lib/libraryName' Consider using the '--user' option or check the permissions."` : `python3 -m pip install -user "package name"` 로 해결
- 설치 시 `"libpcre.so.1 not found"` : 설치 명령어에 `sudo` 붙여서 해결
- [uwsgi 실행 시 wsgi.py를 못찾는 에러 ("Django+Apache ModuleNotFoundError: No module named 'wsgi.py'")](https://stackoverflow.com/questions/42973991/djangoapache-modulenotfounderror-no-module-named-myproject/47256068#comment93487493_42981216)
  ⇒ `[settings.py](http://settings.py)`에 wsgi.py의 경로를 sys.path에 추가하는 코드 추가해서 해결한다.
- [django 버전 확인 방법](https://stackoverflow.com/questions/6468397/how-to-check-django-version) : python 실행 후 `import django`, `django.VERSION` 입력
- [프로젝트 내에서 사용하는 static 파일들을 특정 디렉토리로 모으는 방법](https://my-repo.tistory.com/24)
  ⇒ Nginx에서 참조해야 하기 때문에 설정할 필요가 있다. 프로젝트의 settings.py 에 코드 변경을 해줘야할 부분이 있다. 아래와 같은 형식으로 바꿔주면 된다. `STATIC_DIR`은 BASE_DIR(프로젝트의 manage.py 루트)에 생기게 될 static 파일을 모은 디렉토리를 설정할 이름이다. 수정 후 python 콘솔에서 `python manage.py collectstatic` 명령어를 실행하면 된다.
- [**Nginx, uWSGI 사용 시 Permission denied 에러 (가장 해결에 큰 도움이 된 답변)**](https://stackoverflow.com/questions/22071681/permission-denied-nginx-and-uwsgi-socket)

<br>

**Ubuntu 명령어**

- `pkill -f uwsgi -9`
- `ln -s /etc/nginx/sites-available/firstproject /etc/nginx/sites-enabled/`
- `apt-get install <package name>`
- `cp <fileName> <to_directory>` : 파일을 목표 디렉토리에 복사 (이동은 cp 대신 mv)
- 윈도우에서 리눅스 서버로 파일을 업로드 시키는 방법은 이 링크를 참고하면 된다.
  [20. Linux - 윈도우와 리눅스 사이 파일 이동 시키기(SCP, SFTP)](https://whitewing4139.tistory.com/105)

- `sudo systemctl start uwsgi` : ini 파일들과 함께 uwsgi 가동
- `sudo systemctl enable uwsgi` : uwsgi를 service에 등록
- `service uwsgi status`
- `service nginx start`

<br>

### uWSGI & Nginx 이용해서 Django 프로젝트 배포 성공

but.. 클릭 몇번에 ⚠**504 Gateway timeout** 발생

관련하여 참고한 링크는 여기니까 자세한 내용은 [여기](https://blog.lael.be/post/9251)서 공부합시다!

**Gateway**

⇒ 서로 다른 네트워크 연결 시 출입구 역할

결국 **504 Gateway timeout**은 Nginx에서 upstream과의 통신 지연과 관련해서 설정한 시간 제한을 넘기면 발생하는 것이다.

Gateway timeout의 종류는 3가지라고 한다.

- connect_timeout : **upstream 연결 지연 시 발생**
- send_timeout : 프록시 연결 후 **데이터 전송 지연 시 발생** (ex. 사용자 파일 업로드 속도가 매우 느림)
- read_timeout : 대부분의 원인이다. 응답 요청이 정상적이지만 **응답 지연 시 발생**
  요청(request)은 전송된 상태라 upstream과의 연결을 강제로 끊어도 프로세스는 계속 동작한다. (그래서 터미널도 멈추고 AWS 인스턴스 중지도 엄청 오래 걸렸었다..)

**해결방법**

⇒ 프록시 설정, celery 등 너무 어려워서 일단 [시간 제한을 확장시키는 방법](https://stackoverflow.com/questions/32816975/504-gateway-time-out-uwsgi-nginx-django-application)으로 해결했다.

- Proxy_pass
- uWSGI_pass
- 내부 처리 시간이 긴 경우 celery를 사용하라는 답변도 있었다.

<br>

---

참고)

- [Django Admin(관리자) 계정 찾기 및 비밀번호 변경](https://kitle.xyz/post/58/)
- [Django 테이블 초기화(테이블 데이터 모두 삭제)](https://ohdowon064.tistory.com/316)
- [[WebServer][nginx]로그 보고 수정하기-(3)](https://kamang-it.tistory.com/entry/WebServernginx로그들-보기)
- [https://cjh5414.github.io/how-to-deploy-django-uwsgi-nginx-in-ubuntu/](https://cjh5414.github.io/how-to-deploy-django-uwsgi-nginx-in-ubuntu/)
