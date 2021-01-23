---
layout: post
title: "[CNN]용어학습"
tags: [Machine learning, ai, cnn]
use_math: true
comments: true
---

CNN(Convolutional Neural Network) : 이미지의 공간 정보를 유지한 상태로 학습이 가능한 모델

- 추출한 이미지의 특징을 모으고 강화하는 Pooling 레이어
- 필터를 공유 파라미터로 사용한다. 그래서 일반 인공 신경망과 비교하여 학습 파라미터가 매우 적음.

CNN은 이렇게 나눌 수 있다.

**CNN = Feature extraction + Classification**

(이미지의 특징을 추출하는 부분 + 클래스를 분류하는 부분)

**Feature extraction**

Convolution Layer와 Pooling Layer를 여러 겹 쌓는 형태로 구성됨.

Convolution Layer는 입력 데이터에 필터를 적융하고 활성화 함수를 반영하는 필수 요소이다.

그 다음 위치하는 Pooling Layer는 선택적인 레이어이다.

(CNN 마지막 부분에는 이미지 분류를 위한 Fully Connected 레이어가 추가됩니다. 이미지의 특징을 추출하는 부분과 이미지를 분류하는 부분 사이에 이미지 형태의 데이터를 배열 형태로 만드는 Flatten 레이어가 위치 합니다.)

**관련 용어 종류**

- Convolution(합성곱)
- 채널(Channel)
- 필터(Filter)
- 커널(Kernel)
- 스트라이드(Strid)
- 패딩(Padding)
- 피처 맵(Feature Map)
- 액티베이션 맵(Activation Map)
- 풀링(Pooling) 레이어

### Convolution (합성곱)

합성곱 연산은 입력데이터를 필터가 순회하며 합성곱을 게산하고, 그 계산 결과를 이용하여 Feature map을 만드는 과정을 통해 이해하는게 편하다.

![합성곱 처리 절차](http://deeplearning.stanford.edu/wiki/images/6/6c/Convolution_schematic.gif)
합성곱 처리 절차<br>출처:http://deeplearning.stanford.edu/wiki/index.php/Feature_extraction_using_convolution

<br>

### Channel(채널)

이미지 데이터로 예를 들면, 컬러 이미지는 각 RGB 값을 실수로 표현했기 때문에 3차원데이터이고, 3개의 채널로 구성된다. (반면 흑백은 2차원 데이터, 1개 채널)

Convolution Layer에 유입되는 입력 데이터에는 한개 이상의 필터가 적용되는데, 필터는 Feature Map의 채널이 된다. n개의 필터 적용 시 출력 데이터는 n개의 채널을 갖게 된다.

<br>

### Filter & Stride

필터는 이미지의 특징을 찾아내기 위한 공용 파라미터이자 CNN에서 학습의 대상이다.

Filter를 Kernel이라고 하기도 하며, CNN에서 둘은 같은 의미이다.

입력 데이터를 지정된 간격으로 순회하며 채널별로 합성곱을 하고 모든 채널(컬러의 경우 3개)의 합성곱의 합을 Feature Map로 만든다. 필터는 지정된 간격으로 이동하면서 전체 입력데이터와 합성곱을 하여 Feature Map을 만든다. 여기서 지정된 간격이 바로 **Stride** 이다.

![Feature Map 과정](https://taewanmerepo.github.io/2018/01/cnn/filter.jpg)
Feature Map 과정<br>
(출처:http://taewan.kim/post/cnn/#fnref:2)

<br>

![멀티 채널 입력 데이터에 필터를 적용한 합성곱 계산 절차](https://taewanmerepo.github.io/2018/01/cnn/conv2.jpg)
멀티 채널 입력 데이터에 필터를 적용한 합성곱 계산 절차<br> (출처:http://taewan.kim/post/cnn/#fnref:2)

<br>

### Padding(패딩)

Filter와 Stride로 인해 Convolution Layer에서 Feature Map의 크기는 입력데이터보다 작다. 레이어의 출력 데이터가 줄어드는 것을 방지하기위해 패딩을 사용함.

입력 데이터의 외곽에 지정 픽셀만큼 특정 값으로 채우는 것을 패딩이라고 하는데, 보통 0으로 설정함.

<br>

### Pooling 레이어

풀링 레이어는 컨볼류션 레이어의 출력 데이터를 입력으로 받아서 출력 데이터(Activation Map)의 크기를 줄이거나 특정 데이터를 강조하는 용도로 사용된다.

- Max Pooling
- Average Pooling
- Min Pooling

풀링 레이어를 처리하는 방법의 종류는 이렇고(CNN에서는 보통 Max Pooling), 보통 Pooling 크기와 Stride를 같은 크기로 설정해서 모든 원소가 한 번씩만 처리 되도록 설정한다.

Pooling Layer에는 학습대상 파라미터가 없고, 여길 통과하면 행렬의 크기가 감소하며 채널 수 변경은 없다.

'Convolution 레이어 출력 데이터 크기 산정' 부분은 기반 지식이 더 갖추어진 다음에 읽어야 할 듯 하다..

<br>

---

**참고링크**

- [CNN, Convolutional Neural Network 요약](http://taewan.kim/post/cnn/#fnref:2)
