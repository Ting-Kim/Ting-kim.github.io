---
layout: post
title: "[3rd_week-day1]Python으로 데이터 다루기 I - numpy"
tags: [ai, numpy, git]
use_math: true
comments: true
---

## **Git 기초**

```bash
git init
git add <file_name> # 파일을 깃에 추가 (unstaged -> staged 상태)
git status
git commit -m "커밋 메시지"
```

<br>

---

### 에러 경우

---

git add 시

"warning: :LF will be replaced by CRLF in example.py.

The file will have its original line endings in your working directory" 오류가 발생했다.

이에 대한 해결방법은 [https://blog.jaeyoon.io/2018/01/git-crlf.html](https://blog.jaeyoon.io/2018/01/git-crlf.html) 에 나와있다.

결론은

> > > git config --global core.autocrlf true

를 추가해주면 된다. **Git의 core.autocrlf** 기능을 키는 것인데, '\-\-global' 은 시스템 전체에 적용하겠다는 의미이다. 해당 프로젝트에만 적용하고 싶을 땐 빼면 된다.

---

터미널 입력 시 파일명에 공백이 있을 경우 문제

[https://github.com/rangyu/TIL/blob/master/ubuntu/터미널-입력-시-파일명에-공백이-있을-경우-문제.md](https://github.com/rangyu/TIL/blob/master/ubuntu/%ED%84%B0%EB%AF%B8%EB%84%90-%EC%9E%85%EB%A0%A5-%EC%8B%9C-%ED%8C%8C%EC%9D%BC%EB%AA%85%EC%97%90-%EA%B3%B5%EB%B0%B1%EC%9D%B4-%EC%9E%88%EC%9D%84-%EA%B2%BD%EC%9A%B0-%EB%AC%B8%EC%A0%9C.md)

---

```bash
git branch -v
git branch <branch_name>
git checkout <branch_name> # <branch_name> 브랜치로 이동
git merge <branch_name> # 현재 작업중인 Branch에 원하는 <branch_name>의 브랜치를 병합
git branch -d <branch_name> # git의 Branch 삭제하기
```

**Github**는 로컬저장소와 상호작용을 하면서 분산시스템을 이루는 것이다.

```bash
git remote add <별칭> <원격저장소 주소>
# 별칭에는 보통 origin 을 적는다.
# origin https://github.com/Ting-Kim/example.git

git push <remote_repo_name> <branch_name>

# github가 최근에 정책이 바껴서 master 이름을 main으로 바꿔야 적용하기 수월하다.
git branch -M main

git clone <원격저장소 주소> <별칭> # 로컬저장소에 Github Repository를 복제함

```

---

# Numpy로 연산하기

### Vector와 Scalar 사이의 연산

벡터의각 원소에 대해서 연산을 진행
<br>

<center>$x = \begin{pmatrix}
1 \\
2 \\
3
\end{pmatrix}, \; c=5$</center>

```python
import numpy as np
x = np.array([1,2,3])
c = 5

print("더하기 : {}".format(x+c))
print("빼기 : {}".format(x-c))
print("곱하기 : {}".format(x*c))
print("나누기 : {}".format(x/c))
```

<br>

### Vector와 Vector 사이의 연산

벡터와 **같은 인덱스**끼리 연산이 진행!!!
<br>

<center>$y=\begin{bmatrix}1 \\ 3 \\ 5\end{bmatrix} \; z=\begin{bmatrix}2 \\ 9 \\ 20\end{bmatrix}$</center>

```python
y = np.array([1,3,5])
z = np.array([2,9,20])

print("더하기 : {}".format(y+z))
print("빼기 : {}".format(y-z))
print("곱하기 : {}".format(y*z))
print("나누기 : {}".format(y/z))
```

<br>

### Array의 Indexing

Python의 List에서는 W[0][0]으로 표현했다면
numpy.array() 에서는 W[0,0] 으로 표현한다.

<center>$W=\begin{bmatrix} 1 && 2 && 3 && 4 \\ 5 && 6 && 7 && 8 \\ 9 && 10 && 11 && 12 \end{bmatrix}$</center>

```python
W = np.array([[1,2,3,4], [5,6,7,8],[9,10,11,12]])
print(W[1,3])
```

<br>

## Array Slicing

python의 리스트와 유사하게 진행한다

_2, 3 -> [0:2]_
<br>
_6, 7 -> [1:3]_

```python
print(W[0:2,1:3])
# [[2 3]
#  [6 7]]
print(W[:, 1:3])
# [[ 2  3]
# [ 6  7]
# [10 11]]
```

<br>

## Array의 Broadcasting

Numpy가 연산을 진행하는 특수한 방법!

- 기본적으로 같은 Type의 data에 대해서만 연산이 적용 가능
- 하지만 만약에 피연산자가 연산 가능하도록 변환이 가능하다면 연산이 가능하다.
- 이를 **Broadcasting** 이라고 한다.

### 1. M by N, M by 1

```python
a=np.array([[1,2,3],[4,5,6],[7,8,9]])
x=np.array([0,1,0])
x = x[:, None] # x를 전치

print(a+x)
# [[1 2 3]
#  [5 6 7]
#  [7 8 9]]
```

### 2. M by N, 1 by N

```python
y = np.array([0,1,-1])

print(a * y)
# [[ 0  2 -3]
#  [ 0  5 -6]
#  [ 0  8 -9]]
```

### 3. M by 1, 1 by N

```python
t = np.array([1,2,3]) # 열벡터로 바꿔줘야 한다.
t = t[:,None] # 전치(Transpose)

u = np.array([2,0,-2])
print(t + u)

# 1 1 1   2 0 -2
# 2 2 2 + 2 0 -2
# 3 3 3   2 0 -2

# [[ 3  1 -1]
#  [ 4  2  0]
#  [ 5  3  1]]
```

```python
print("더하기 : {}".format(M+N))
print("빼기 : {}".format(M-N))
print("곱하기 : {}".format(M*N))
print("나누기 : {}".format(M/N))

# 더하기 : [[ 2  4 12]
#  [ 5  7 15]
#  [ 8 10 18]]
# 빼기 : [[ 0  0 -6]
#  [ 3  3 -3]
#  [ 6  6  0]]
# 곱하기 : [[ 1  4 27]
#  [ 4 10 54]
#  [ 7 16 81]]
# 나누기 : [[1.         1.         0.33333333]
#  [4.         2.5        0.66666667]
#  [7.         4.         1.        ]]
```

<br>

## Numpy로 선형대수 지식 끼얹기

### A. basics

### 영벡터 (행렬)

- 원소가 모두 0인 벡터(행렬)
- `np.zeros(dim)` 을 통해 생성, dim은 값, 혹은 튜플(,)

```python
np.zeros(2)
# array([0., 0.])
```

<br>

### 일벡터(일행렬)

- 원소가 모두 1인 벡터(행렬)
- `np.ones(dim)`을 통해 생성, dim은 값, 튜플(,)

```python
np.ones((2,3))
# array([[1., 1., 1.],
#        [1., 1., 1.]])
```

<br>

### 대각행렬(diagonal)

- Main Diagonal을 제외한 성분이 0인 행렬
- `np.diag((main_diagonals))` 을 통해 생성할 수 있음.

```python
np.diag((2,5))
# array([[2, 0],
#        [0, 5]])
```

<br>

### 항등행렬(identity matrix)

- main diagonal == 1인 diagonal matrix(대각 행렬)
- `np.eye(n, (dtype=int, uint, float, complex, ...))` 를 사용

```python
np.eye(2) # default dtype=float
# array([[1., 0.],
#        [0., 1.]])
```

<br>

### 행렬곱(dot product)

- 행렬 간에 정의되는 곱 연산
- `np.dot()` , `@`를 사용

```python
mat_1 = np.array([[1,4], [2,3]])
mat_2 = np.array([[7,9],[0,6]])

mat_1.dot(mat_2)
# array([[ 7, 33],
#        [14, 36]])

mat_1 @ mat_2
# array([[ 7, 33],
#        [14, 36]])
```

<br>

### 트레이스(trace)

- Main diagonal의 합
- `np.trace()` 를 사용

```python
arr = np.array([[1,2,3], [4,5,6],[7,8,9]])
arr.trace()
# 15

np.eye(2).trace()
# 2.0
```

<br>

### 행렬식(determinant)

- 행렬을 대표하는 값 중 하나
- 선형변환 과정에서 Vector의 Scaling 척도
- `np.linalg.det()` 으로 계산

```python
arr_2 = np.array([[2,3], [1,6]])
np.linalg.det(arr_2)
# 9.000000000000002

arr_3 = np.array([[1,4,7],[2,5,8],[3,6,9]])
np.linalg.det(arr_3)
# 0.0
```

<br>

### 역행렬(Inverse matrix)

- 행렬 A에 대해서 $AB=BA=I$를 만족하는 행렬 B
- `np.linalg.inv()`

```python
mat_inv = np.linalg.inv(arr_2)
mat_inv
# array([[ 0.66666667, -0.33333333],
#        [-0.11111111,  0.22222222]])

# 확인
arr_2 @ mat_inv
# array([[1., 0.],
#        [0., 1.]])
```

<br>

### 고유값과 고유벡터(eigenvalue and eigenvector)

- 정방행렬 A에 대해 $Ax=\lambda x$를 만족하는 상수 $\lambda$와 이에 대응하는 벡터
- `np.linalg.eig()`를 이용하여 계산

```python
mat = np.array([[2,0,-2], [1,1,-2],[0,0,1]])
np.linalg.eig(mat)
# (array([1., 2., 1.]),
#  array([[0.        , 0.70710678, 0.89442719],
#         [1.        , 0.70710678, 0.        ],
#         [0.        , 0.        , 0.4472136 ]]))
```

#### validation

```python
eig_val, eig_vec = np.linalg.eig(mat)
eig.vec
# array([[0.        , 0.70710678, 0.89442719],
#        [1.        , 0.70710678, 0.        ],
#        [0.        , 0.        , 0.4472136 ]])

mat@eig_vec[:,0] # Ax
# array([0., 1., 0.])

eig_val[0] * eig_vec[:,0] # (lambda)x
# array([0., 1., 0.])

mat@eig_vec[:, 1]
# array([1.41421356, 1.41421356, 0.        ])

eig_val[1] * eig_vec[:, 1]
# array([1.41421356, 1.41421356, 0.        ])
```
