---
layout: post
title: "[3rd_week-day2]Python으로 데이터 다루기 Ⅱ - pandas"
tags: [ai, pandas, dataframe]
use_math: true
comments: true
---

# Pandas

### Prerequisite : Table

- 행과 열을 이용해서 데이터를 저장하고 관리하는 자료구조(컨테이너)
- 주로 행은 개체, 열은 속성을 나타낸다.

### Pandas 시작하기

`import pandas` 를 통해서 진행

```python
import pandas as pd
```

Ⅱ. pandas로 1차원 다루기 - Series

### Series?

- 1-D labeled **array**
- 인덱스를 지정해줄 수 있음

```python
s = pd.Series([1,4,9,16,25])
s
```

    0     1
    1     4
    2     9
    3    16
    4    25
    dtype: int64

```python
t = pd.Series({'one':1, 'two':2, 'three':3, 'four':4,'five':5})
t
```

    one      1
    two      2
    three    3
    four     4
    five     5
    dtype: int64

## Series + Numpy

- Series는 ndarray와 유사하다!

```python
t[1]
```

    2

```python
t[1:3]
```

    two      2
    three    3
    dtype: int64

```python
s[s > s.median()] # 자기 자신의 median(중앙값) 보다 큰 값들만 가져온다
```

    3    16
    4    25
    dtype: int64

```python
s[[4,1,1]]
```

    4    25
    1     4
    1     4
    dtype: int64

```python
import numpy as np

np.exp(s)
```

    0    2.718282e+00
    1    5.459815e+01
    2    8.103084e+03
    3    8.886111e+06
    4    7.200490e+10
    dtype: float64

## Series + dict

- series는 dict와 유사하다

```python
t
```

    one      1
    two      2
    three    3
    four     4
    five     5
    dtype: int64

```python
'six' in t
```

    False

```python
'five' in t
```

    True

```python
t.get('five')
```

    5

```python
t.get('six')
```

```python
t.get('six', 0) # 딕셔너리와 유사
```

    0

## Series에 이름 붙이기

- `name` 속성을 가지고 있다.
- 처음 Series를 만들 때 이름을 붙일 수 있습니다.

```python
s = pd.Series(np.random.randn(5), name="random_nums") # randn() = 가우시안 분포 상에서의 임의의 난수 생성
s
```

    0   -0.412482
    1    0.938129
    2    1.569555
    3   -0.181722
    4    0.555711
    Name: random_nums, dtype: float64

```python
s.name = "임의의 난수"
```

```python
s
```

    0   -0.412482
    1    0.938129
    2    1.569555
    3   -0.181722
    4    0.555711
    Name: 임의의 난수, dtype: float64

## 3. Pandas로 2차원 데이터 다루기 - dataframe

### dataframe이란?

- 2-D labeled **table**
- 인덱스를 지정할 수도 있음

```python
d = {"height":[1,2,3,4], "weight":[30,40,50,60]}
df = pd.DataFrame(d)
df
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>height</th>
      <th>weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>30</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>40</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>50</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>

```python
## dtype 확인
df.dtypes
```

    height    int64
    weight    int64
    dtype: object

### From CSV to dataframe

Comma Serperative Value - CSV 파일
csv -> dataframe

- Comma Separated Value를 DataFrame으로 생성해줄 수 잇다.
- `read_csv()`를 이용

```python
# 동일 경로에 country_wise_latest.csv가 존재하면:

covid = pd.read_csv("./country_wise_latest.csv")

covid
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country/Region</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>New cases</th>
      <th>New deaths</th>
      <th>New recovered</th>
      <th>Deaths / 100 Cases</th>
      <th>Recovered / 100 Cases</th>
      <th>Deaths / 100 Recovered</th>
      <th>Confirmed last week</th>
      <th>1 week change</th>
      <th>1 week % increase</th>
      <th>WHO Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>36263</td>
      <td>1269</td>
      <td>25198</td>
      <td>9796</td>
      <td>106</td>
      <td>10</td>
      <td>18</td>
      <td>3.50</td>
      <td>69.49</td>
      <td>5.04</td>
      <td>35526</td>
      <td>737</td>
      <td>2.07</td>
      <td>Eastern Mediterranean</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>4880</td>
      <td>144</td>
      <td>2745</td>
      <td>1991</td>
      <td>117</td>
      <td>6</td>
      <td>63</td>
      <td>2.95</td>
      <td>56.25</td>
      <td>5.25</td>
      <td>4171</td>
      <td>709</td>
      <td>17.00</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>27973</td>
      <td>1163</td>
      <td>18837</td>
      <td>7973</td>
      <td>616</td>
      <td>8</td>
      <td>749</td>
      <td>4.16</td>
      <td>67.34</td>
      <td>6.17</td>
      <td>23691</td>
      <td>4282</td>
      <td>18.07</td>
      <td>Africa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Andorra</td>
      <td>907</td>
      <td>52</td>
      <td>803</td>
      <td>52</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>5.73</td>
      <td>88.53</td>
      <td>6.48</td>
      <td>884</td>
      <td>23</td>
      <td>2.60</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Angola</td>
      <td>950</td>
      <td>41</td>
      <td>242</td>
      <td>667</td>
      <td>18</td>
      <td>1</td>
      <td>0</td>
      <td>4.32</td>
      <td>25.47</td>
      <td>16.94</td>
      <td>749</td>
      <td>201</td>
      <td>26.84</td>
      <td>Africa</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>182</th>
      <td>West Bank and Gaza</td>
      <td>10621</td>
      <td>78</td>
      <td>3752</td>
      <td>6791</td>
      <td>152</td>
      <td>2</td>
      <td>0</td>
      <td>0.73</td>
      <td>35.33</td>
      <td>2.08</td>
      <td>8916</td>
      <td>1705</td>
      <td>19.12</td>
      <td>Eastern Mediterranean</td>
    </tr>
    <tr>
      <th>183</th>
      <td>Western Sahara</td>
      <td>10</td>
      <td>1</td>
      <td>8</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10.00</td>
      <td>80.00</td>
      <td>12.50</td>
      <td>10</td>
      <td>0</td>
      <td>0.00</td>
      <td>Africa</td>
    </tr>
    <tr>
      <th>184</th>
      <td>Yemen</td>
      <td>1691</td>
      <td>483</td>
      <td>833</td>
      <td>375</td>
      <td>10</td>
      <td>4</td>
      <td>36</td>
      <td>28.56</td>
      <td>49.26</td>
      <td>57.98</td>
      <td>1619</td>
      <td>72</td>
      <td>4.45</td>
      <td>Eastern Mediterranean</td>
    </tr>
    <tr>
      <th>185</th>
      <td>Zambia</td>
      <td>4552</td>
      <td>140</td>
      <td>2815</td>
      <td>1597</td>
      <td>71</td>
      <td>1</td>
      <td>465</td>
      <td>3.08</td>
      <td>61.84</td>
      <td>4.97</td>
      <td>3326</td>
      <td>1226</td>
      <td>36.86</td>
      <td>Africa</td>
    </tr>
    <tr>
      <th>186</th>
      <td>Zimbabwe</td>
      <td>2704</td>
      <td>36</td>
      <td>542</td>
      <td>2126</td>
      <td>192</td>
      <td>2</td>
      <td>24</td>
      <td>1.33</td>
      <td>20.04</td>
      <td>6.64</td>
      <td>1713</td>
      <td>991</td>
      <td>57.85</td>
      <td>Africa</td>
    </tr>
  </tbody>
</table>
<p>187 rows × 15 columns</p>
</div>

## Pandas 활용 1. 일부분만 관찰하기

`head(n)`:처음 n개의 데이터 참조

```python
# 위에서부터 5개를 관찰하는 방법(함수)

covid.head(5)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country/Region</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>New cases</th>
      <th>New deaths</th>
      <th>New recovered</th>
      <th>Deaths / 100 Cases</th>
      <th>Recovered / 100 Cases</th>
      <th>Deaths / 100 Recovered</th>
      <th>Confirmed last week</th>
      <th>1 week change</th>
      <th>1 week % increase</th>
      <th>WHO Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>36263</td>
      <td>1269</td>
      <td>25198</td>
      <td>9796</td>
      <td>106</td>
      <td>10</td>
      <td>18</td>
      <td>3.50</td>
      <td>69.49</td>
      <td>5.04</td>
      <td>35526</td>
      <td>737</td>
      <td>2.07</td>
      <td>Eastern Mediterranean</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>4880</td>
      <td>144</td>
      <td>2745</td>
      <td>1991</td>
      <td>117</td>
      <td>6</td>
      <td>63</td>
      <td>2.95</td>
      <td>56.25</td>
      <td>5.25</td>
      <td>4171</td>
      <td>709</td>
      <td>17.00</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>27973</td>
      <td>1163</td>
      <td>18837</td>
      <td>7973</td>
      <td>616</td>
      <td>8</td>
      <td>749</td>
      <td>4.16</td>
      <td>67.34</td>
      <td>6.17</td>
      <td>23691</td>
      <td>4282</td>
      <td>18.07</td>
      <td>Africa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Andorra</td>
      <td>907</td>
      <td>52</td>
      <td>803</td>
      <td>52</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>5.73</td>
      <td>88.53</td>
      <td>6.48</td>
      <td>884</td>
      <td>23</td>
      <td>2.60</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Angola</td>
      <td>950</td>
      <td>41</td>
      <td>242</td>
      <td>667</td>
      <td>18</td>
      <td>1</td>
      <td>0</td>
      <td>4.32</td>
      <td>25.47</td>
      <td>16.94</td>
      <td>749</td>
      <td>201</td>
      <td>26.84</td>
      <td>Africa</td>
    </tr>
  </tbody>
</table>
</div>

`tail(n)`: 마지막 n개의 데이터를 참조

```python
# 아래에서부터 5개를 관찰하는 방법(함수)

covid.tail(5)

```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country/Region</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>New cases</th>
      <th>New deaths</th>
      <th>New recovered</th>
      <th>Deaths / 100 Cases</th>
      <th>Recovered / 100 Cases</th>
      <th>Deaths / 100 Recovered</th>
      <th>Confirmed last week</th>
      <th>1 week change</th>
      <th>1 week % increase</th>
      <th>WHO Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>182</th>
      <td>West Bank and Gaza</td>
      <td>10621</td>
      <td>78</td>
      <td>3752</td>
      <td>6791</td>
      <td>152</td>
      <td>2</td>
      <td>0</td>
      <td>0.73</td>
      <td>35.33</td>
      <td>2.08</td>
      <td>8916</td>
      <td>1705</td>
      <td>19.12</td>
      <td>Eastern Mediterranean</td>
    </tr>
    <tr>
      <th>183</th>
      <td>Western Sahara</td>
      <td>10</td>
      <td>1</td>
      <td>8</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10.00</td>
      <td>80.00</td>
      <td>12.50</td>
      <td>10</td>
      <td>0</td>
      <td>0.00</td>
      <td>Africa</td>
    </tr>
    <tr>
      <th>184</th>
      <td>Yemen</td>
      <td>1691</td>
      <td>483</td>
      <td>833</td>
      <td>375</td>
      <td>10</td>
      <td>4</td>
      <td>36</td>
      <td>28.56</td>
      <td>49.26</td>
      <td>57.98</td>
      <td>1619</td>
      <td>72</td>
      <td>4.45</td>
      <td>Eastern Mediterranean</td>
    </tr>
    <tr>
      <th>185</th>
      <td>Zambia</td>
      <td>4552</td>
      <td>140</td>
      <td>2815</td>
      <td>1597</td>
      <td>71</td>
      <td>1</td>
      <td>465</td>
      <td>3.08</td>
      <td>61.84</td>
      <td>4.97</td>
      <td>3326</td>
      <td>1226</td>
      <td>36.86</td>
      <td>Africa</td>
    </tr>
    <tr>
      <th>186</th>
      <td>Zimbabwe</td>
      <td>2704</td>
      <td>36</td>
      <td>542</td>
      <td>2126</td>
      <td>192</td>
      <td>2</td>
      <td>24</td>
      <td>1.33</td>
      <td>20.04</td>
      <td>6.64</td>
      <td>1713</td>
      <td>991</td>
      <td>57.85</td>
      <td>Africa</td>
    </tr>
  </tbody>
</table>
</div>

### Pandas 활용 2. 데이터 접근하기

- `df['column_name']` or `df.column_name`

```python
covid['Confirmed']
```

    0      36263
    1       4880
    2      27973
    3        907
    4        950
           ...
    182    10621
    183       10
    184     1691
    185     4552
    186     2704
    Name: Confirmed, Length: 187, dtype: int64

```python
covid.Active # column_name에 공백이 포함된 경우는 사용이 불가하다
```

    0      9796
    1      1991
    2      7973
    3        52
    4       667
           ...
    182    6791
    183       1
    184     375
    185    1597
    186    2126
    Name: Active, Length: 187, dtype: int64

## Honey Tip! dataFrame의 각 column은 "Series" 이다.

```python
type(covid['Confirmed']) #
```

    pandas.core.series.Series

```python
covid['Confirmed'][1]
```

    4880

```python
covid['Confirmed'][1:5]
```

    1     4880
    2    27973
    3      907
    4      950
    Name: Confirmed, dtype: int64

### Pandas 활용 3. "조건"을 이용해서 데이터 접근하기

```python
# 신규 확진자가 100명이 넘는 나라를 뽑아보자

covid[covid["New cases"] > 100]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country/Region</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>New cases</th>
      <th>New deaths</th>
      <th>New recovered</th>
      <th>Deaths / 100 Cases</th>
      <th>Recovered / 100 Cases</th>
      <th>Deaths / 100 Recovered</th>
      <th>Confirmed last week</th>
      <th>1 week change</th>
      <th>1 week % increase</th>
      <th>WHO Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>36263</td>
      <td>1269</td>
      <td>25198</td>
      <td>9796</td>
      <td>106</td>
      <td>10</td>
      <td>18</td>
      <td>3.50</td>
      <td>69.49</td>
      <td>5.04</td>
      <td>35526</td>
      <td>737</td>
      <td>2.07</td>
      <td>Eastern Mediterranean</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>4880</td>
      <td>144</td>
      <td>2745</td>
      <td>1991</td>
      <td>117</td>
      <td>6</td>
      <td>63</td>
      <td>2.95</td>
      <td>56.25</td>
      <td>5.25</td>
      <td>4171</td>
      <td>709</td>
      <td>17.00</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>27973</td>
      <td>1163</td>
      <td>18837</td>
      <td>7973</td>
      <td>616</td>
      <td>8</td>
      <td>749</td>
      <td>4.16</td>
      <td>67.34</td>
      <td>6.17</td>
      <td>23691</td>
      <td>4282</td>
      <td>18.07</td>
      <td>Africa</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Argentina</td>
      <td>167416</td>
      <td>3059</td>
      <td>72575</td>
      <td>91782</td>
      <td>4890</td>
      <td>120</td>
      <td>2057</td>
      <td>1.83</td>
      <td>43.35</td>
      <td>4.21</td>
      <td>130774</td>
      <td>36642</td>
      <td>28.02</td>
      <td>Americas</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Australia</td>
      <td>15303</td>
      <td>167</td>
      <td>9311</td>
      <td>5825</td>
      <td>368</td>
      <td>6</td>
      <td>137</td>
      <td>1.09</td>
      <td>60.84</td>
      <td>1.79</td>
      <td>12428</td>
      <td>2875</td>
      <td>23.13</td>
      <td>Western Pacific</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>177</th>
      <td>United Kingdom</td>
      <td>301708</td>
      <td>45844</td>
      <td>1437</td>
      <td>254427</td>
      <td>688</td>
      <td>7</td>
      <td>3</td>
      <td>15.19</td>
      <td>0.48</td>
      <td>3190.26</td>
      <td>296944</td>
      <td>4764</td>
      <td>1.60</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>179</th>
      <td>Uzbekistan</td>
      <td>21209</td>
      <td>121</td>
      <td>11674</td>
      <td>9414</td>
      <td>678</td>
      <td>5</td>
      <td>569</td>
      <td>0.57</td>
      <td>55.04</td>
      <td>1.04</td>
      <td>17149</td>
      <td>4060</td>
      <td>23.67</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>180</th>
      <td>Venezuela</td>
      <td>15988</td>
      <td>146</td>
      <td>9959</td>
      <td>5883</td>
      <td>525</td>
      <td>4</td>
      <td>213</td>
      <td>0.91</td>
      <td>62.29</td>
      <td>1.47</td>
      <td>12334</td>
      <td>3654</td>
      <td>29.63</td>
      <td>Americas</td>
    </tr>
    <tr>
      <th>182</th>
      <td>West Bank and Gaza</td>
      <td>10621</td>
      <td>78</td>
      <td>3752</td>
      <td>6791</td>
      <td>152</td>
      <td>2</td>
      <td>0</td>
      <td>0.73</td>
      <td>35.33</td>
      <td>2.08</td>
      <td>8916</td>
      <td>1705</td>
      <td>19.12</td>
      <td>Eastern Mediterranean</td>
    </tr>
    <tr>
      <th>186</th>
      <td>Zimbabwe</td>
      <td>2704</td>
      <td>36</td>
      <td>542</td>
      <td>2126</td>
      <td>192</td>
      <td>2</td>
      <td>24</td>
      <td>1.33</td>
      <td>20.04</td>
      <td>6.64</td>
      <td>1713</td>
      <td>991</td>
      <td>57.85</td>
      <td>Africa</td>
    </tr>
  </tbody>
</table>
<p>82 rows × 15 columns</p>
</div>

```python
covid["WHO Region"].unique() # 범주의 종류를 리스트 형태로 번환해줌
covid[covid["WHO Region"] == "South-East Asia"]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country/Region</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>New cases</th>
      <th>New deaths</th>
      <th>New recovered</th>
      <th>Deaths / 100 Cases</th>
      <th>Recovered / 100 Cases</th>
      <th>Deaths / 100 Recovered</th>
      <th>Confirmed last week</th>
      <th>1 week change</th>
      <th>1 week % increase</th>
      <th>WHO Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13</th>
      <td>Bangladesh</td>
      <td>226225</td>
      <td>2965</td>
      <td>125683</td>
      <td>97577</td>
      <td>2772</td>
      <td>37</td>
      <td>1801</td>
      <td>1.31</td>
      <td>55.56</td>
      <td>2.36</td>
      <td>207453</td>
      <td>18772</td>
      <td>9.05</td>
      <td>South-East Asia</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Bhutan</td>
      <td>99</td>
      <td>0</td>
      <td>86</td>
      <td>13</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>0.00</td>
      <td>86.87</td>
      <td>0.00</td>
      <td>90</td>
      <td>9</td>
      <td>10.00</td>
      <td>South-East Asia</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Burma</td>
      <td>350</td>
      <td>6</td>
      <td>292</td>
      <td>52</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>1.71</td>
      <td>83.43</td>
      <td>2.05</td>
      <td>341</td>
      <td>9</td>
      <td>2.64</td>
      <td>South-East Asia</td>
    </tr>
    <tr>
      <th>79</th>
      <td>India</td>
      <td>1480073</td>
      <td>33408</td>
      <td>951166</td>
      <td>495499</td>
      <td>44457</td>
      <td>637</td>
      <td>33598</td>
      <td>2.26</td>
      <td>64.26</td>
      <td>3.51</td>
      <td>1155338</td>
      <td>324735</td>
      <td>28.11</td>
      <td>South-East Asia</td>
    </tr>
    <tr>
      <th>80</th>
      <td>Indonesia</td>
      <td>100303</td>
      <td>4838</td>
      <td>58173</td>
      <td>37292</td>
      <td>1525</td>
      <td>57</td>
      <td>1518</td>
      <td>4.82</td>
      <td>58.00</td>
      <td>8.32</td>
      <td>88214</td>
      <td>12089</td>
      <td>13.70</td>
      <td>South-East Asia</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Maldives</td>
      <td>3369</td>
      <td>15</td>
      <td>2547</td>
      <td>807</td>
      <td>67</td>
      <td>0</td>
      <td>19</td>
      <td>0.45</td>
      <td>75.60</td>
      <td>0.59</td>
      <td>2999</td>
      <td>370</td>
      <td>12.34</td>
      <td>South-East Asia</td>
    </tr>
    <tr>
      <th>119</th>
      <td>Nepal</td>
      <td>18752</td>
      <td>48</td>
      <td>13754</td>
      <td>4950</td>
      <td>139</td>
      <td>3</td>
      <td>626</td>
      <td>0.26</td>
      <td>73.35</td>
      <td>0.35</td>
      <td>17844</td>
      <td>908</td>
      <td>5.09</td>
      <td>South-East Asia</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Sri Lanka</td>
      <td>2805</td>
      <td>11</td>
      <td>2121</td>
      <td>673</td>
      <td>23</td>
      <td>0</td>
      <td>15</td>
      <td>0.39</td>
      <td>75.61</td>
      <td>0.52</td>
      <td>2730</td>
      <td>75</td>
      <td>2.75</td>
      <td>South-East Asia</td>
    </tr>
    <tr>
      <th>167</th>
      <td>Thailand</td>
      <td>3297</td>
      <td>58</td>
      <td>3111</td>
      <td>128</td>
      <td>6</td>
      <td>0</td>
      <td>2</td>
      <td>1.76</td>
      <td>94.36</td>
      <td>1.86</td>
      <td>3250</td>
      <td>47</td>
      <td>1.45</td>
      <td>South-East Asia</td>
    </tr>
    <tr>
      <th>168</th>
      <td>Timor-Leste</td>
      <td>24</td>
      <td>0</td>
      <td>0</td>
      <td>24</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>24</td>
      <td>0</td>
      <td>0.00</td>
      <td>South-East Asia</td>
    </tr>
  </tbody>
</table>
</div>

## Pandas 활용 4. 행을 기준으로 데이터 접근하기

```python
# 예시 데이터 - 도서관 정보

books_dict = {"Available":[True, True, False], "Location":[102, 215, 323], "Genre":["Programming", "Physics", "Math"]}

books_df = pd.DataFrame(books_dict, index=["버그란 무엇인가", "두근두근 물리학","미분해줘 홈즈"])

books_df
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Available</th>
      <th>Location</th>
      <th>Genre</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>버그란 무엇인가</th>
      <td>True</td>
      <td>102</td>
      <td>Programming</td>
    </tr>
    <tr>
      <th>두근두근 물리학</th>
      <td>True</td>
      <td>215</td>
      <td>Physics</td>
    </tr>
    <tr>
      <th>미분해줘 홈즈</th>
      <td>False</td>
      <td>323</td>
      <td>Math</td>
    </tr>
  </tbody>
</table>
</div>

### 인덱스를 이용해서 가져오기 : `.loc[row, col]`

```python
books_df.loc["두근두근 물리학"]
```

    Available       True
    Location         215
    Genre        Physics
    Name: 두근두근 물리학, dtype: object

```python
# "미분해줘 홈즈 책이 대출 가능한지?"

books_df.loc["미분해줘 홈즈", "Available"]
```

    False

### 숫자 인덱스를 이용해서 가져오기 : `.iloc[rowidx, colidx`

```python
books_df.iloc[1,1]
```

    215

```python
books_df.iloc[1,:]
```

    Available       True
    Location         215
    Genre        Physics
    Name: 두근두근 물리학, dtype: object

## Pandas 활용 5. groupby

- Split : 특정한 "기준"을 바탕으로 DataFrame을 분할
- Apply : 통계함수 - sum(), mean(), median(), - 을 적용해서 각 데이터를 압축
- Combine : Apply된 결과를 바탕으로 새로운 Series를 생성 (group_key : applied_value)

`.groupby()`

```python
covid.head(5)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country/Region</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>New cases</th>
      <th>New deaths</th>
      <th>New recovered</th>
      <th>Deaths / 100 Cases</th>
      <th>Recovered / 100 Cases</th>
      <th>Deaths / 100 Recovered</th>
      <th>Confirmed last week</th>
      <th>1 week change</th>
      <th>1 week % increase</th>
      <th>WHO Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>36263</td>
      <td>1269</td>
      <td>25198</td>
      <td>9796</td>
      <td>106</td>
      <td>10</td>
      <td>18</td>
      <td>3.50</td>
      <td>69.49</td>
      <td>5.04</td>
      <td>35526</td>
      <td>737</td>
      <td>2.07</td>
      <td>Eastern Mediterranean</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>4880</td>
      <td>144</td>
      <td>2745</td>
      <td>1991</td>
      <td>117</td>
      <td>6</td>
      <td>63</td>
      <td>2.95</td>
      <td>56.25</td>
      <td>5.25</td>
      <td>4171</td>
      <td>709</td>
      <td>17.00</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>27973</td>
      <td>1163</td>
      <td>18837</td>
      <td>7973</td>
      <td>616</td>
      <td>8</td>
      <td>749</td>
      <td>4.16</td>
      <td>67.34</td>
      <td>6.17</td>
      <td>23691</td>
      <td>4282</td>
      <td>18.07</td>
      <td>Africa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Andorra</td>
      <td>907</td>
      <td>52</td>
      <td>803</td>
      <td>52</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>5.73</td>
      <td>88.53</td>
      <td>6.48</td>
      <td>884</td>
      <td>23</td>
      <td>2.60</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Angola</td>
      <td>950</td>
      <td>41</td>
      <td>242</td>
      <td>667</td>
      <td>18</td>
      <td>1</td>
      <td>0</td>
      <td>4.32</td>
      <td>25.47</td>
      <td>16.94</td>
      <td>749</td>
      <td>201</td>
      <td>26.84</td>
      <td>Africa</td>
    </tr>
  </tbody>
</table>
</div>

```python
# WHO Region 별 확진자수

# 1. covid에서 확진자 수 column만 추출한다
# 2. 이를 covid의 WHO Region을 기준으로 groupby한다.

covid_by_region = covid['Confirmed'].groupby(by=covid["WHO Region"])
covid_by_region
# <pandas.core.groupby.generic.SeriesGroupBy object at 0x000001F5C5DD5730> - split 만 적용된 상태

covid_by_region.sum()

```

    WHO Region
    Africa                    2129.5
    Americas                  7340.0
    Eastern Mediterranean    28575.0
    Europe                   12191.0
    South-East Asia           3333.0
    Western Pacific           1009.5
    Name: Confirmed, dtype: float64

```python
# 국가당 감염자 수

covid_by_region.mean()
```

    WHO Region
    Africa                    15066.812500
    Americas                 252551.028571
    Eastern Mediterranean     67761.090909
    Europe                    58920.053571
    South-East Asia          183529.700000
    Western Pacific           18276.750000
    Name: Confirmed, dtype: float64

```python
# 중앙값

covid_by_region.median()

```

    WHO Region
    Africa                    2129.5
    Americas                  7340.0
    Eastern Mediterranean    28575.0
    Europe                   12191.0
    South-East Asia           3333.0
    Western Pacific           1009.5
    Name: Confirmed, dtype: float64
