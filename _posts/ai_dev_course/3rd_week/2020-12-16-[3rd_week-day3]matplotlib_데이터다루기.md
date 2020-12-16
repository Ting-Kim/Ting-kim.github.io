---
layout: post
title: "[3rd_week-day3]Python으로 데이터 다루기 Ⅱ - matplotlib"
tags: [ai, matplotlib, seaborn]
use_math: true
comments: true
---

## 1. Matplotlib 시작하기

- 파이썬의 데이터 시각화 라이브러리
- `%matplotlib inline`을 통해서 활성화

cf) 라이브러리(numpy, pandas, ..) vs 프레임워크(django, ..)

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

%matplotlib inline
```

## 2. Case Study with Arguments

```python
plt.plot([1,2,-3,4,-5]) # 실제 plotting 하는 함수
plt.show() # plt를 확인하는 명령
```

![png](matplotlib/output_4_0.png)

### Figsize : Figure(도면)의 크기 조절

```python
plt.figure(figsize=(5,5)) # plotting을 할 도면을 선언

plt.plot([0,1,2,3,4]) # 실제 plotting 하는 함수 # y=x
plt.show() # plt를 확인하는 명령
```

![png](matplotlib/output_6_0.png)

### 2차함수 그래프 with plot()

```python
# 리스트를 이용해서 1차함수 y=x를 그려보면:

plt.plot([0,1,2,3,4])
plt.show()
```

![png](matplotlib/output_8_0.png)

```python
# numpy.array를 이용해서 함수 그래프 그리기
# y = x^2
x = np.array([1,2,3,4,5,6,7,8,9,10]) # 정의역
y = np.array([1,4,9,16,25,36,49,64,81,100])# f(x)

plt.plot(x,y)
plt.show()
```

![png](matplotlib/output_9_0.png)

```python
# np.arange(a,b,c) c: 0.01 (실수 가능함)

x = np.arange(-10,10,0.01)
plt.plot(x, x**2)
plt.show()
```

![png](matplotlib/output_10_0.png)

```python
# x, y축에 설명 추가하기

x = np.arange(-10,10,0.01)

plt.xlabel("x value")
plt.ylabel("f(x) value")

plt.plot(x, x**2)
plt.show()
```

![png](matplotlib/output_11_0.png)

```python
# x, y축에 범위를 설정하기

x = np.arange(-10,10,0.01)

plt.xlabel("x value")
plt.ylabel("f(x) value")

###
plt.axis([-5, 5, 0, 25]) # [x_min, x_max, y_min, y_max]
###


plt.plot(x, x**2)
plt.show()
```

![png](matplotlib/output_12_0.png)

```python
# x, y축에 눈금 설정하기

x = np.arange(-10,10,0.01)

plt.xlabel("x value")
plt.ylabel("f(x) value")
plt.axis([-5, 5, 0, 25]) # [x_min, x_max, y_min, y_max]

###
plt.xticks([i for i in range(-5, 5, 1)])
plt.yticks([i for i in range(0, 24, 3)])
# plt.grid()  # matlab에서 했던것 같아서 해보니 기능이 있다 . . . 신기
###

plt.plot(x, x**2)
plt.show()
```

![png](matplotlib/output_13_0.png)

```python
# 그래프에 title 달기

x = np.arange(-10,10,0.01)

plt.xlabel("x value")
plt.ylabel("f(x) value")
plt.axis([-5, 5, 0, 25]) # [x_min, x_max, y_min, y_max]
plt.xticks([i for i in range(-5, 5, 1)])
plt.yticks([i for i in range(0, 24, 3)])

###
plt.title("y = x^2 graph")
###

plt.plot(x, x**2)
plt.show()
```

![png](matplotlib/output_14_0.png)

```python
# 그래프에 범례 달기

x = np.arange(-10,10,0.01)

plt.xlabel("x value")
plt.ylabel("f(x) value")
plt.axis([-5, 5, 0, 25]) # [x_min, x_max, y_min, y_max]
plt.xticks([i for i in range(-5, 5, 1)])
plt.yticks([i for i in range(0, 24, 3)])
plt.title("y = x^2 graph")

###
plt.plot(x, x**2, label="trend")
plt.legend() # 범례
###

plt.show()
```

![png](matplotlib/output_15_0.png)

## 3. Matplotlib Case Study

### 꺾은선 그래프 (Plot)

- `.plot()`
- 시계열도에서 가장 많이 사용한다.

```python
x = np.arange(20) # 0~19
y = np.random.randint(0, 20, 20)# 난수를 20번 생성

plt.plot(x, y)

# y축을 20까지 보이게 하고싶다면?
plt.axis([0,20,0,20])
plt.yticks([0, 5, 10, 15, 20])

plt.show()

# Extra : y축을 20까지 보이게 하고싶다면?, y축을 "5"단위로 보이게 하고 싶다면?
# .axis(), .yticks()
```

![png](matplotlib/output_18_0.png)

### 산점도 (Scatter Plot)

- `.scatter()`
- 보통 별개인 변수를 다룰 때 사용
- 이를 통해서 상관관계가 있는지 확인해볼 수 있다.

```python
plt.scatter(x,y)
plt.show()
```

![png](matplotlib/output_20_0.png)

### 박스 그림 (Box Plot)

- `plt.boxplot()`
- 수치형 데이터에 대한 정보 (Q1, Q2, Q3, min, max)

```python
plt.boxplot(y)
plt.title("Box plot of y")
plt.show()

# Extra : Plot의 title을 "Box plot of y"
```

![png](matplotlib/output_22_0.png)

### 막대 그래프 (Bar Plot)

- `.bar()`
- 범주형 데이터의 "값"과 그 값의 크기를 직사각형으로 나타낸 그림

```python
plt.bar(x,y)

plt.xticks(np.arange(0, 20, 1))
plt.show()

# Extra : xticks를 올바르게 처리해봅시다.

```

![png](matplotlib/output_24_0.png)

### cf) Histogram

- `.hist()`

```python
# 도수분포를 직사각형의 막대 형태로 나타냈다.
# "계급"으로 나타낸 것이 특징 : 0, 1, 2가 아니라 0~2까지의 "범주형" 데이터로 구성 후 그림을 그림

plt.hist(y, bins=np.arange(0,20,2))
plt.xticks(np.arange(0,20,2))

plt.show()

# Extra : xticks를 올바르게 고쳐봅시다.
```

![png](matplotlib/output_26_0.png)

### 원형 그래프 (Pie Chart)

- 데이터에서 전체에 대한 부분의 비율을 부채꼴로 나타낸 그래프
- 다른 그래프에 비해서 **비율** 확인에 용이
- `.pie()`

```python
z = [100, 300, 200, 400]

plt.pie(z, labels=[100, 200, 300, 400])
plt.show()
```

![png](matplotlib/output_28_0.png)
