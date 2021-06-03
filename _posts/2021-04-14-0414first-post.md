---
title: "SWEA 1244-Python"
excerpt: "문제풀이"

categories:
  - Blog
tags:
  - algorithm
  - python
last_modified_at: 2021-04-14 21:38:00-05:00

comments : true
---





















※ 최적의 풀이가 아닐수 있으므로 참고만해주세요!  틀린점 댓글로 지적해주세요~! 

문제: [SWEA 1244최대상금](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15Khn6AN0CFAYD&categoryId=AV15Khn6AN0CFAYD&categoryType=CODE&problemTitle=1244&orderBy=FIRST_REG_DATETIME&selectCodeLang=ALL&select-1=&pageSize=10&pageIndex=1)



### 코드

```python
def myfunction(arr, cnt):
    if cnt == change:
        global res
        temp = int(''.join(arr))
        res = max(res, temp)
        return
    for g in range(len(arr) - 1):
        for f in range(g + 1, len(arr)):
            arr[f], arr[g] = arr[g], arr[f]
            temp = ''.join(arr)
            if memo.get((temp, cnt), True):
                memo[(temp, cnt)] = 0
                myfunction(arr, cnt + 1)
            arr[f], arr[g] = arr[g], arr[f]


for tc in range(int(input())):
    num, change = input().split()
    num = list(num)
    change = int(change)
    res = 0
    memo = {}
    myfunction(num, 0)
    print("#{} {}".format(tc + 1, res))
```



### 선세줄요약

- 쓸모없는 반복을 줄이기 위한 가지치기
-  `dict`형태로 `memoization`  하기

- 이게 왜 난이도3 밖에...?

### 본론

최대6자리의 숫자의 자리를 서로 바꾸어 최대값을 구하는 문제이지만

- tc마다 제공되는 자리 교환횟수를 반드시 이행
- 교환했던 자리 재 교환 가능

이 두가지때문에 골머리를 꽤나 썩였다.

```python
#첫제출(시간초과)
def myfunction(arr, cnt):
    if cnt == change:
        global res
        temp = int(''.join(arr))
        res = max(res, temp)
        return
    tarr = arr.copy()
    for g in range(len(arr)-1):
        for f in range(g+1,len(arr)):
            if g != f:
                tarr[f], tarr[g] = tarr[g], tarr[f]
                temp = ''.join(tarr)
                if memo.get((temp, cnt), True):
                    memo[(temp, cnt)] = 0
                    myfunction(tarr, cnt + 1)
                tarr[f], tarr[g] = tarr[g], tarr[f]


for tc in range(int(input())):
    num, change = input().split()
    num = list(num)
    change = int(change)
    res = 0
    memo = {}
    myfunction(num, 0)
    print("#{} {}".format(tc + 1, res))
```

처음엔 아무생각없이 이렇게 코딩하였다.

재귀문 종료 조건을 `cnt == change`로 설정하고

이중반복문에선 자기 자신과 교환하는 경우를 제외(`f!=g`)하면 자리교환, `cnt+1`만하여 재귀하였다. 

그리고 재귀가 종료될때의 값을 `res`에 저장하여`max()`함수로 최대값을 출력하였다.

하지만 시간이 너~무 오래걸린다는 문제점이 있었다.

최대6자리숫자의 10번 교환하는 tc에는 경우의 수가 **576,650,390,625개**((30/2)^10)이기 때문이다.

(숫자 선택 * 교환할 자리 선택/중복^ 10번)

특히 `777770 5`의 경우에는 몇번을 교환하던 숫자가 같은 경우가 많기에 중복실행을 막아야한다

그럼 순회중 이미 실행했던 숫자에 대해서만 `memoization`을 하고 제외시켜야하는가?

그렇지 않다. 몇번 교환했는지 까지 memo해주어야한다.

메모조건이 까다롭기에 `dict`에 `memo`하기로했다.

그렇기에 통과 코드에서 함수부분만 살펴보면

```python
def myfunction(arr, cnt):
    if cnt == change:#교환횟수를 다 채웠을때
        global res
        temp = int(''.join(arr))#arr의 값을 temp에 넣어주고
        res = max(res, temp)#결과값과 temp중 더 큰값을 res에 넣어준다
        return
    for g in range(len(arr) - 1):#교환할 자리 택1
        for f in range(g + 1, len(arr)):#나머지자리 택1
            arr[f], arr[g] = arr[g], arr[f]#선 교환
            temp = ''.join(arr)
            if memo.get((temp, cnt), True):#memo에 (숫자,교환횟수)가 있는가? 없으면 True를 반환하라
                memo[(temp, cnt)] = 0#없다면 memo에 입력.다음에는 거르게됨
                myfunction(arr, cnt + 1)#교환된 arr와 횟수+1를 인자로 해서 재귀
            arr[f], arr[g] = arr[g], arr[f]#원상복구
```





여기서 이중for문이 왜

```python
    for g in range(len(arr)):
        for f in range(len(arr)):
            if g != f:
```

이게 아니고

```python
    for g in range(len(arr)-1):
        for f in range(g+1,len(arr)):
```

이거인가? 

문제에선 이미 교환한 자리수도다시교환 가능하며, 앞의 자리와도 교환이 가능하다 라고 되어있는데

왜 첫번째께 아니지??? 싶지만

어차피 두번째 자리와 네번째 자리를 교환하나 네번째 자리와 두번째 자리를 교환하나

상관없기때문이다.

2-4, 4-2, 2-4, 4-2 이런식으로 교환해하는 경우도

어차피 재귀하면 2-4,2-4,2-4,2-4 이런식으로 교환하기때문에

첫번째껄 쓰면 햇던작업을 또 해야한다는 문제가 있다.

실제로 위 두코드는 실행횟수가 정확하게 2배 차이가난다.

그리고 `len(arr)-1`해준것은 쓸모없는 작업을 한번이라도 더 줄이기 위함이다.

### 결론

재귀를 통해 조합 구현할 줄 알아야 한다.   