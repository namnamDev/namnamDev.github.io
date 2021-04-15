---
title: "SWEA 5189 전자카트-Python"
excerpt: "문제풀이"

categories:
  - Blog
tags:
  - algorithm
  - python
last_modified_at: 2021-04-15 17:22:00-05:00

comments : true
---



※ 최적의 풀이가 아닐수 있으므로 참고만해주세요!  틀린점 댓글로 지적해주세요~! 

문제: [파이썬 SW문제해결 응용_구현 - 02 완전 검색](https://swexpertacademy.com/main/learn/course/subjectDetail.do?courseId=AVuPDYSqAAbw5UW6&subjectId=AWUYDrI61lYDFAVT#)



### 코드

```python
def myfunction(num, arr, val):
    if num == N - 1:
        global res
        val += board[arr[-1]][0]
        res = min(val, res)
        return
    else:
        for i in range(1, N):
            if not visited[i]:
                visited[i] = 1
                myfunction(num + 1, arr + [i], val + board[arr[-1]][i])
                visited[i] = 0


for tc in range(int(input())):
    N = int(input())
    board = [list(map(int, input().split())) for _ in range(N)]
    temp = list(range(N))
    visited = [0] * N
    visited[0] = 1
    res = 100 * N * N
    myfunction(0, [0], 0)
    print("#{} {}".format(tc + 1, res))
```



### 선세줄요약

- 경우의 수와 dfs문제를 합친듯한 문제
- 경우의 수를 산출하는 방법만으로 불필요한 재귀(가지치기)를 할수있다.
- 

### 본론

이미 방문한 지점은 visited처리하며, 경로의 값들을 인자로 해서 재귀시켜줌.

단, 처음과 끝은 출발지점이여야 하므로 경우의 수를 산출할때 출발지점은 제외시켜야함.

그리고 재귀가 종료되는 시점에서 초기에 돌아오는 값을 추가해줌.