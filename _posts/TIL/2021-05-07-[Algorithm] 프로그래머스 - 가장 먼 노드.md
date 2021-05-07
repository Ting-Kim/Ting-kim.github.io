# [코테] 프로그래머스 - 가장 먼 노드

```python
"""
    프로그래머스 레벨 3 - 가장 먼 노드
    https://programmers.co.kr/learn/courses/30/lessons/49189
"""
from collections import deque

def solution(n, edge):
    answer = set()

    graph = [set([]) for _ in range(n+1)]

    # edge 기준으로 그래프 만들기
    for edge_ in edge:
        graph[edge_[0]].add(edge_[1])
        graph[edge_[1]].add(edge_[0])

    visited = [False] * (n+1)

    def bfs(start):
        queue = deque([(start, 0)]) # 노드 번호, 거친 간선 수
        visited[start] = True

        while queue:
            node, distance = queue.popleft()

            for num in graph[node]:
                if visited[num] is True:
                    answer.add((node, distance))
                    continue

                queue.append((num, distance+1))
                visited[num] = True

    # bfs 돌리고 탐색 종료 마다 (노드 번호, 간선 개수)를 리스트에 추가
    bfs(1)

    # 리스트를 lambda x[1] 기준으로 내림차순 정렬
    answer_list = sorted(answer, key=lambda x:x[1], reverse=True)

    # 최대값만 노드갯수 셈
    max_distance = answer_list[0][1]

    answer_count = 0
    for node_info in answer_list:
        if node_info[1] != max_distance:
            break
        answer_count += 1

    return answer_count

n = 6
vertex = [[3, 6], [4, 3], [3, 2], [1, 3], [1, 2], [2, 4], [5, 2]]
print(solution(n, vertex))
```