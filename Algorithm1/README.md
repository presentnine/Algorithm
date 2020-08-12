# 알고리즘 1

## 비트 조작

 데이터의 저장에 있어서 이진수를 사용하는 컴퓨터에선 모든 연산이 이진법을 바탕으로 이루어지기에, 정수연산에 있어서 **비트 연산을 사용하면 훨씬 효율적**이다. C++ 기준 비트 연산에 사용하는 연산자로는 **논리 연산자 4종류**와 **시프트 연산자 2종류**가 있다.

 근래에는 하드웨어의 발달로 오히려 유지, 보수가 쉽게 프로그래머가 잘 알아볼 수 있는 코드로 작성하는게 좋다. 하지만 여전히 내장 메모리가 작은 하드웨어 등에서는 비트 연산을 사용한다.



### 논리 연산자

#### AND 연산자(&)

* **X&Y** 형태
* X와 Y의 비트열에서 **동일한 위치에 있는 각 비트**에 대해 AND 연산을 수행

```c++
int a=5;
int b=6;
int c=5&6; //0101&0110=0100 -> c=4
```

* 두 정수의 단순 연산 보다는 AND 연산 특성 상 **한 쪽이 0이면 결과값이 0이기에**, 비트열 속에서 특정 부분의 결과값을 0으로 초기화할 때 사용한다. **Masking 연산**이라고도 한다.

#### OR 연산자(|)

* **X|Y** 형태
* X와 Y의 비트열에서 **동일한 위치에 있는 각 비트**에 대해 OR 연산을 수행

```c++
int a=5;
int b=6;
int c=5|6; //0101|0110=0111 -> c=7
```

* AND 연산과 마찬가지로 직접 두 정수의 단순 연산 보다는 주로 비트열 속 **특정 부분을 1로 세팅**할 때 사용

#### XOR 연산자(^)

* **X^Y** 형태
* X와 Y의 비트열에서 **동일한 위치에 있는 각 비트**에 대해 XOR 연산을 수행

```c++
int a=5;
int b=6;
int c=5^6; //0101^0110=0011 -> c=3
```

* 특정 비트를 반전시킬 때 사용

#### NOT 연산자(~)

* **~X** 형태
* X의 비트열에서 **각 비트**에 대해 NOT연산을 수행

```c++
int a=4;
int b=~a; //~0100=1011 -> b=-5
```

* 1의 보수를 구할 때 사용



### 시프트 연산자

#### 왼쪽 시프트 연산자(<<)

* **X<<Y** 형태
* X의 비트열을 왼쪽으로 Y칸씩 이동, **X * 2^Y**의 값이 된다.

```c++
int a=8;
int b=2;
int c=a<<b; //00001000<<2=00100000 -> c=32
```

* 비워진 비트 위치에 대해서 0으로 채워지며, 넘어가는 비트들은 무시된다.

#### 오른쪽 시프트 연산자(>>)

* **X>>Y** 형태
* X의 비트열을 오른쪽으로 Y칸씩 이동, **X / 2^Y**의 값이 된다.

```c++
int a=8;
int b=2;
int c=a>>b; //00001000>>2=00000010 -> c=2
```

* C/C++ 기준 비트열의 값이 부호 있는 숫자인 경우, 오른쪽 시프트에 대하여 부호 비트를 채우는 데 기존 부호 비트 값을 그대로 이용한다. **(음수의 경우 1이 들어온다)**
  * [Visuial Studio 2019 속 시프트 연산자](https://docs.microsoft.com/ko-kr/cpp/cpp/left-shift-and-right-shift-operators-input-and-output?view=vs-2019)



## 완전 탐색

 말 그대로 문제 속 **모든 경우의 수에 대해 탐색을 진행**하는 방법으로, 컴퓨터의 빠른 연산 능력을 이용하여 **시간에 대한 제약을 제외하면** 문제를 쉽게 풀어낼 수 있다. 다양한 방법이 존재하지만, 주로 **반복문을 이용한 방법**과 **재귀함수를 이용한 방법**을 사용한다.



#### 반복문 활용

* 반복해야 하는 코드에 대해 **for문, while문** 등을 사용하여 반복
* 프로그래머 입장에선 쉽게 작성이 가능

* ex) 1부터 n까지의 합을 계산하는 코드

```c++
int sum(int n){
    int ret=0;
    for(int i=1;i<=n;i++)
        ret+=i;
    return ret;
}
```



#### 재귀함수(Recursive function) 활용

* 자신이 수행할 작업을 **유사한 형태의 조각으로 나눠** 먼저 한 조각을 수행하는 방법, 이후 나머지 조각들도 해결해 나감
* 이를 위해 **함수 내부에 자기 자신을 다시 한 번 호출**함
* 따라서, **무한 루프에 빠질 위험**이 있어 함수가 return할 **기저 조건(Base Case)** 가 필요
* ex) 1부터 n까지의 합을 계산하는 코드

```c++
int sum(int n){
    if(n==0) //Base Case
        return 0;
    
    return n + sum(n-1);//Recursive Call
}
```

* 재귀함수 자체가 시스템 스택에 부하를 주기 때문에, 재귀함수를 최적화한 방법인 **꼬리 재귀(Tail Recursion)** 방법 또한 존재

  ```c++
  int sumTail(int n, int acc){
      if(n==0)
          return acc;
      return sumTail(n - 1, acc + n);
  }
  ```



번외) [프로그래머스 라면공장](https://programmers.co.kr/learn/courses/30/lessons/42629)의 완전 탐색 방법

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int search(int stock, vector<int> dates, vector<int> supplies, int k, int index){//완전탐색
    int result = 987654321;

    if (stock >= k)
        return 0;

    for (int i = index + 1; i < dates.size(); i++) {
        if (stock >= dates[i]) {
            result = min(result, search(stock + supplies[i], dates, supplies, k, i) + 1);
        }
        else {
            break;
        }
    }

    return result;
}

int solution(int stock, vector<int> dates, vector<int> supplies, int k) {
    int answer;
    answer = search(stock, dates, supplies, k, -1);
    return answer;
}
```



