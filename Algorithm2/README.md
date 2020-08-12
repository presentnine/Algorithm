# 알고리즘 2

## 동적 프로그래밍

 최적화 문제를 연구하는 수학이론에서 나온 말로, 동적(dynamic)이란 말이 들어가지만 실상 내용과는 아무런 관련은 없다. **완전 탐색**을 위해 재귀 호출을 적용하는 과정에서, 또는 큰 문제를 작은 문제로 나눠 해결해 나가는 과정에서 **반복되는 계산**에 대해 여러 번 계산을 하는 것이 아닌 **한 번만 계산을 하고 그 결과를 재활용함**으로써 속도의 향상을 얻는 법이다.

![피보나치](C:\Users\admin\Desktop\MD 폴더\알고리즘\피보나치.png)



 그러한 과정에서 **분할 정복**과 알고리즘이 비슷하지만, 분할 정복은 문제를 나누는 과정에서 계산이 **중복되는 부분이 발생하지 않는다**. 반면에 **동적 프로그래밍**의 경우 **중복되는 부분이 발생**하여 해당 부분에 대해 이전 결과를 재활용한다.

 계산을 하는 과정에서 작은 문제를 풀고 큰 문제로 넘어가는 **상향식 접근법(bottom-up)**, 큰 문제를 나눠 작은 문제로 나누고 작은 문제를 해결해 나가는 **하향식 접근법(top-down)** 방식으로 구분되기도 한다.

* **상향식 접근법(Bottom-up)**: 재귀로 구현
* **하향식 접근법(Top-down)**: 반복문으로 구현



### 메모제이션(Memoization)

 동적 프로그래밍을 위해 사용하는 방식으로, 한 번 계산한 결과를 **메모리에 저장**해 두었다가 필요할 때 그 값을 재사용하는 방식을 말한다.

ex) [백준 - RGB거리](https://www.acmicpc.net/problem/1149)

```c++
int N;
vector<vector<int>> price(1000, vector<int>(3,0));
vector<vector<int>> cache(1000, vector<int>(3, 0)); //메모제이션

int solve(int row, int col) {
	int best = 1000000;

	if (cache[row][col] >  0) //이전 값 재활용
		return cache[row][col];

	for (int i = 0; i < 3; i++) {
		if (col != i)
			best = min(best, price[row][col] + solve(row + 1, i));
	}

	return cache[row][col] = best; //계산 값을 메모리에 저장
}
```

ex) [백준-LCS](https://www.acmicpc.net/problem/9251)

```c++
vector<vector<int>> cache2(1000, vector<int>(1000, 1)); //메모제이션

int solve(int fPos, int sPos) {
	int best = 1;

	if (cache2[fPos][sPos] > 1 || fPos == N - 1 || sPos == M - 1) //이전 값 재활용
		return cache2[fPos][sPos];

	for (int i = fPos + 1; i < N; i++)
		for (int j = sPos + 1; j < M; j++)
			if (first[i] == second[j]) {
				best = max(best, solve(i, j) + 1);
				break;
			}

	return cache2[fPos][sPos] = best; //계산 값을 메모리에 저장
}
```



####  적용법

* 문제를 **하나 이상의 변수를 사용**하여 하나의 **함수**로 표현
* 하나의 **부분 문제의 결과를 담을 배열(캐시)을 생성**
  * **Bottom-up** 방식의 경우 반복문을 통해 배열의 값을 채워나감, 이후 부분 문제를 확장시키며 배열의 값을 활용하여 더 큰 문제를 쉽게 해결
  * **Top-down** 방식의 경우 재귀를 통해 문제를 작은 문제로 나눠가며 결과 값을 계산, 결과를 배열에 저장하여 이를 나중에 재사용하여 큰 문제를 쉽게 해결
