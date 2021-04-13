---
title: "백준1000java/python"
excerpt: "문제풀이"

categories:
  - Blog
tags:
  - algorithm
last_modified_at: 2021-04-12 20:38:00-05:00

comments : true

---
아마 내 백준 첫문제였을것이다.

비전공자이기에 어떻게 알고리즘을 공부해야하는지 조차 감도 안잡히는 시기였다.

지금은 그나마 SSAFY 교육을 통해 그나마 감잡고 공부할수있다.. 

```java
//java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		Scanner sc= new Scanner(System.in);
		int A= sc.nextInt();
		int B= sc.nextInt();
		System.out.println( A + B );
	}

}
```

지금은 파이썬을 배우기에 파이썬으로도 풀어보았다.

```python
#python
A,B=map(int,input().split())
print(A+B)
```

자바로는 몇줄이나 되는 코드지만 파이썬으로는 단 두줄이면 된다.

아니, 한줄이면 된다.

```python
print(eval('+'.join(input()))) #dwcy2306님의 풀이
```

숏코드 기준으로 검색했을때 고작 30B의 길이다.

자바도 정말 훌륭한 언어지만, 알고리즘 문제풀 때 만큼은 파이썬이 좋다..