## 클라우드 기초

- IDC(Internet Data Center)
  - 서버 운영에 필요한 공간, 네트워크, 유지 보수 등의 서비스를 제공
  - 효율적인 자원과 적은 비용으로 사용할 수 있엇지만, 대부분의 IDC의 서버 임대는 계약을 통해 일정 기간 임대하는 **유연성이 떨어지는 구조**이다.

<br>

**Cloud Computing의 장점?**

4차 산업혁명 시대에서 빅데이터의 수집, 저장, 분석을 위한 방대한 컴퓨팅 자원과 인공지능 개발을 위한 고성능 컴퓨터를 사용할 수 있는 IT 환경이 마련하다.

- 속도
- 접근성
- 확장성
- 생산성
- 보안, 안정성
- 측정가능성

운용 모델

- 퍼블릭(Public)
- 프라이빗(Private)
  - 보안적인 부분은 좋지만, 폐쇄적이라 개발에 사용하는데 어려움이 있음
  - 최근에는 글로벌 클라우드 사업자들이 IT기술만ㅁ 패키지 형태로 판매하기도 했다.
- 하이브리드(Hybrid)
  - 고객의 핵심 시스템은 내부에 두면서도 외부의 클라우드를 활용

<br>

클라우드 서비스 제공 모델

- On-Premises
- IaaS
- PaaS
- SaaS

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/4th_week-day2_1.png?raw=true](2020-12-22-%5B4th_week-day2%5D%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A1%E1%84%8B%E1%85%AE%E1%84%83%E1%85%B3_%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC_%E1%84%86%E1%85%A5%E1%84%89%E1%85%B5%E1%86%AB%E1%84%85%E1%85%A5%2084622e9d7b7d43f3a44287e32e4177a9/Untitled.png)

비유하면 ..

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/4th_week-day2_2.png?raw=true](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/4th_week-day2_2.png?raw=true)

클라우드 제공 사업자 - AWS, GCP, Naver Cloud Platform, Azure 등

<br>

## AWS EC2 가상환경 연결 및 접속

macOS SSH 터미널에서는 `chmod 400 <pem file>`, `ssh -i "<pem file>" ubuntu@<퍼블릭 IPv4 DNS>` 를 명령어로 입력하면 바로 접속이 되는데.. 윈도우 환경에서는 이게 안되서 굉장히 애먹었다.

권한 설정을 잘 해주어야 한다. pem file이 들어있는 디렉토리의 권한을 보이는대로 수정해준다.

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/4th_week-day2_3.png?raw=true](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/4th_week-day2_3.png?raw=true)

<br>

여기서 주의해야 할 것은 아래와 같이 사용자를 선택해줄 때 '**이름 확인**'을 꼭 해주고 등록해야 한다.

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/4th_week-day2_4.png?raw=true](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/4th_week-day2_4.png?raw=true)

<br>

AWS 인스턴스 아웃바운드 설정 - EC2 인스턴스 접속에 성공하면 삭제해준다.

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/4th_week-day2_5.png?raw=true](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/4th_week-day2_5.png?raw=true)

<br>

**일반적인 머신러닝 프로세스**

문제 정의 → 데이터 준비 단계(원천 데이터 가공, 모델 학습을 위한 Feature Engineering) → 모델 학습 → 모델 검증(배포 가능한 모델인지 확인) → Trained Model File 배포 → 지속적인 모니터링

저장된 모델 파일(Trained Model File)을 불러와서 Run/Predict 하는 것이 이번 프로젝트 내용이다.

<br>

### Model Serving

모델 학습 → 학습된 모델 저장(Serialization) → Web Framework를 통해 모델을 배포

- **모든 과정에서 연속성이 유지 되어야 한다.**
  - 모델 성능 저하 및 실행에 문제가 있을 수 있기 때문
- **Serving Model 에서는 상황에 맞게 다양한 Serving Framework를 사용해야 한다.**

<br>

**Serialization & De-serialization**

- ML/DL model object를 Serialization을 통해 Disk(전송하고 불러올 수 있는 형태)에 write하여 변환 할 수 있다.
- De-Serialization을 통해 메모리(Python 혹은 다른 환경)에 불러올 수 있다.
- 이 때, 두 과정이 같은 방식으로 진행되어야 가능하다.

API 서비스 제공할 때 Handler를 통해서 일련의 과정들을 수행할 수 할 수 있다. 코드의 관리 및 유지 보수 측면에서 Handler를 이용하여 구현하는게 좋다.

<br>

**Model Serving을 위한 Framework**

- 일반적으로 TensorFlow serving, TorchServe, TensorRT 프레임워크를 사용한다.
- 웹 프레임워크와 서빙 프레임워크가 내부 통신을 통해서 커뮤니케이션 하는것이 일반적인 형태이다.

<br>

## Save and Load trained model

```python
// aws ec2 인스턴스 연결 후 ubuntu 환경에서..
// 실습 파일 복제
$ conda activate pytorch_p36
(pytorch_p36) $ git clone https://github.com/sackoh/kdt-ai-aws
// "Failed to connect to https://gitlab.com/xxxx/aws_tests_git_1.git port 443: Connection timed out"
// 에러가 발생한다.
```

아래의 아웃바운드 규칙을 설정해 줄 것!

![https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/4th_week-day2_6.png?raw=true](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/4th_week-day2_6.png?raw=true)

<br>

```python
// 참고할 것
https://aws.amazon.com/ko/premiumsupport/knowledge-center/ec2-linux-resolve-ssh-connection-errors/

pip install -r requirements.txt
// 입력 후 설치하는 과정에서 ec2 원격 연결이 끊겼다.. 이후로 자꾸 타임아웃 되어버림..
// cpu 부족으로 인해서 t2.micro -> t2.large로 변경 후 설치했음.
// 그리고 다시 t2.micro로 변경했다..
```

<br>

## Serving_Model - Define inference

**Skeleton of model handler to serve model**

```python
class ModelHandler(BaseHandler): # BaseHandler로부터 상속받을 것을 상속 받고
	def __init__(self): # 초기화 진행
		pass

	def initialize(self, **kwargs):
		pass

	def preprocess(self, data): # 입력된 값에 대해 전처리
		pass

	def inference(self, data): # 불러온 모델에 대해 추론
		pass

	def postprocess(self, data): # 결과에 대해 후처리 보정해준다.
		pass

	def handle(self, data): # 실질적으로 API에서는 이 핸들러를 대부분 호출한다.
		pass
```

handle() : 요청 정보를 받아 적절한 응답을 반환

- 정의된 양식으로데이터가 입력됐는지 확인
- 입력 값에 대한 전처리 및 모델에 입력하기 위한 형태로 변환
- 모델 추론
- 모델 반환값의 후처리 작업
- 결과 반환

<br>

---

참고 링크

- ***

찾아봐야 할 개념

- 코드에 사용된 메서드들에 대한 내용
