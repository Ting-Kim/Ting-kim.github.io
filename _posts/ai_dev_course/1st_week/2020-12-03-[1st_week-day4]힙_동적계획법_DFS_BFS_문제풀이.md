# 2020-12-02-[1st_week-day4]힙_동적계획법_DFS_BFS_문제풀이

### 파이썬에서 힙(라이브러리)을 사용하는 방법

```python
import heapq
heapq.heapify(L)     # L(리스트)로부터 min heap을 구성한다.
m = heapq.heappop(L) # min heap인 L에서 최소값을 삭제하고 반환한다.
heapq.heappush(L,x)  # min heap인 L에 원소 x를 삽입한다.
```

\*힙 자료 구조는 [[1st week-day2]](https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/_posts/ai_dev_course/1st_week/2020-12-02-%5B1st%20week-day2%5D%ED%81%90_%ED%8A%B8%EB%A6%AC_%ED%9E%99.md) 참고

<br>

### 동적계획법 (Dynamic Programming)

⇒ 알고리즘이 진행되면서 **탐색해야 할 범위를 동적으로 결정**하며 탐색 범위를 한정할 수 있다.

동적계획법이 적용된 대표적인 좋은 문제는 **"배낭 문제(Knapsack problem)"**이다. 꼭 별도로 풀어보고 학습해볼 것.