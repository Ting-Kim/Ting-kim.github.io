---
layout: post
title: "[4th_week-day1]Web Application with Flask"
tags: [ai, web, Flask, db connection]
use_math: true
comments: true
---

# Web Application with Flask

- **Flask** 활용 REST API 구축
- **Django** 활용 웹 사이트 구축

## Flask

- Python 기반 마이크로 웹 프레임워크

```python
# (venv) # 가상환경 진입 후
pip freeze # 설치된 모듈들을 보여줌
pip install flask # flask 설치
```

<br>

### REST API (Representational State Transfer)

`"웹 서버가 요청을 응답하는 방법론 중 하나"` - 데이터가 아닌, 자원(Resource)의 관점으로 접근

- HTTP URI를 통해 자원을 명시
- HTTP Method 를 통해 해당 자원에 대한 CRUD 진행

  HTTP Method (같은 URI로 다른 Action 이 가능함)

  - GET
  - POST
  - PUT
  - DELETE

- Stateless (무상태성) - Client의 Context를 서버에서 유지하지 않는다.
  - 요청과 응답을 독립적으로 받아들임

Request & Response 테스트는 **POSTMAN**을 이용한다

POST 메서드의 경우, Request에 대한 정보가 Http 프로토콜의 Body에 담긴다.

<br>

**주요 모듈**

- `from flask import redirect, url_for`
  - redirect()
  - url_for()
- `from flask_sqlalchemy import SQLAlchemy`
  - .query().filter_by().first()
  - .delete()
  - .add()
  - .commit()
  - .rollback()
  - .close()
- `import pymysql`
  - mysql 연동 시 사용 (`app.config["SQLALCHEMY_DATABASE_URI"] = "mysql+pymysql://root:1234@localhost:3306/flask"`)

<br>

## DB(Mysql) 연동해서 커피 메뉴 CRUD 구현하기

```python
'''##app.py
    [week4김태호]day1_보너스 과제 Ⅱ - DB 연동
    커피 메뉴 출력
    - DB : mysql
    - pip install flask_sqlalchemy SQLAlchemy pymysql
    - python console에서 아래 코드 실행 후 flask run
        >>from app import db
        >>db.create_all()
'''

from flask import Flask, jsonify, request, url_for # url_for은 redirect 시 사용
from flask_sqlalchemy import SQLAlchemy
import pymysql
# jsonify는 파이썬의 딕셔너리 타입을 json 형식으로 바꿔줌
# 왜 cmd 통해서 가상환경에 모듈을 설치했는데 에디터 통해서 프로젝트를 열면 package가 없다고 할까?

app = Flask(__name__)

## db info setting
app.config["SQLALCHEMY_DATABASE_URI"] = "mysql+pymysql://root:1234@localhost:3306/flask"
app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False # 추가적인 메모리가 필요한 기능이므로 꺼둔다.

## db set
db = SQLAlchemy(app)
db.init_app(app)

class Menu(db.Model):
    __tablename__= 'menus'
    __table_args__ = {'mysql_collate': 'utf8_general_ci'}

    id = db.Column(db.Integer, primary_key=True, nullable=False, autoincrement=True)
    name=db.Column(db.String(30), nullable=False)
    price=db.Column(db.Integer, nullable=False)

    def __init__(self, name, price):
        self.name = name
        self.price = price

    def __repr__(self):
        # return jsonify("{'id' : %d, 'name' : '%s', 'price' : %d}" % (self.id, self.name, self.price))
        return "{'id' : %d, 'name' : '%s', 'price' : %d}" % (self.id, self.name, self.price)

@app.route('/')
def hello_flask():
    return "Hello World!"

# GET / menus | 자료를 가지고 온다.
@app.route('/menus')
def get_menus():
    def get():
        lst = db.session.query(Menu).all()
        dict_lst = [{'id':x.id, 'name':x.name, 'price':x.price,} for x in lst]
        return dict_lst

    menus = get()

    return jsonify({"menus" : menus})

# POST /menus | 자료를 자원에 추가한다.
@app.route('/menus', methods=['POST'])
def create_menu(): # request가 JSON이라고 가정
    def create(be_create):
        db.session.add(be_create)

    # request에 Client가 Server로 부터 POST 요청할 때 담겨진 자료를 parsing
    request_data = request.get_json()

    ## work with session
    # 입력받은 내용으로 Menu 인스턴스 생성
    menu = Menu(name=request_data['name'], price=request_data['price'])

    try:
        create(menu)
        db.session.commit()

    except:
        db.session.rollback()
        raise

    finally:
        db.session.close()

    return get_menus()

@app.route('/menus/<id>', methods=['PUT'])
def modify_menu(id):
    id = int(id)
    find_menu = db.session.query(Menu).filter_by(id=id).first() # 결과가 없을 시 None 반환

    if find_menu == None:
        return jsonify({"message" : "해당 id가 존재하지 않습니다."})
    request_data = request.get_json()
    find_menu.name = request_data['name']
    find_menu.price = request_data['price']

    return get_menus()

@app.route('/menus/<id>', methods=['DELETE'])
def delete_menu(id):
    def delete(del_menu):
        db.session.delete(del_menu)

    id = int(id)
    find_menu = db.session.query(Menu).filter_by(id=id).first()
    if find_menu == None:
        return jsonify({"message": "해당 id가 존재하지 않습니다."})

    try:
        delete(find_menu)
        db.session.commit()

    except:
        db.session.rollback()
        raise

    finally:
        db.session.close()

    return get_menus()

if __name__ == '__main__': # app.py에서 직접적인 실행파일로 사용될 때 실행하라는 로직
    app.run()
```

<br>

---

참고 링크

- [SQLAlchemy document 1.3](https://docs.sqlalchemy.org/en/13/orm/session_basics.html)
- [sqlalchymy-uickstart](https://flask-sqlalchemy.palletsprojects.com/en/2.x/quickstart/)

<br>

---

찾아봐야 할 개념

- 파이썬 - with ~ as 문법
- [Flask 로깅 관련 문서](http://flask.pocoo.org/docs/1.0/logging/)
