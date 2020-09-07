# 알고리즘 6

## 최대, 최소 찾기 -> 토너먼트 트리(선택 트리)

 각각 **정렬된** 상태로 있는 **배열들을** **하나의 배열로 합칠 때** 사용되는 알고리즘으로, 여러 배열 속 가장 작은 최소값/최대값을 찾아내는데 필요한 **비교 횟수를 줄일 수 있다.** 이 때 선택 트리에는 **승자 트리와 패자 트리** 두 종류가 있다.



### 승자 트리(winner tree)

 각 노드가 2개의 **자식 중 더 작은 노드**를 나타내는 완전 이진 트리로, 결과적으로 **루트 노드는 가장 작은 값**을 가진다.

![winnertree](https://github.com/presentnine/Algorithm/blob/master/Algorithm6/winnertree.png)

* 각 배열에서 값이 **하나씩 삽입**
* 둘 중 **더 작은 키 값**을 가지는 원소가 **올라간다**
* 루트가 결정되는 대로 해당 값을 **출력하고 삭제**
* 삭제된 값 쪽의 배열에서 다음 값을 삽입하고 **트리를 다시 구성**한다.



#### 특징

* 배열의 개수를 k, 총 레코드(값)의 개수를 n이라 하면 모두 병합하는 데 **O(n*log(k))** 만큼의 복잡도를 가진다.



### 패자 트리(loser tree)

 각 노드가 두 자식 노드보다 더 작은 값을 갖는 완전 이진 트리 라는 점은 승자 트리와 방식이 비슷하지만, 트리의 **각 내부 노드에 승자가 아닌 패자(더 큰 값)이 저장**된다. 구성 또한 루트 노드 위에 최상위 노드가 있어 최종 승자를 표시한다.

![losertree](https://github.com/presentnine/Algorithm/blob/master/Algorithm6/losertree.png)

* 각 배열에서 값이 **하나씩 삽입**
* 둘 중 **더 작은 키 값**을 가지는 원소가 **올라간다**
* 이 때 **패자(더 큰 값)** 을 가지는 원소는 해당 노드에 남게 된다.
* 루트 노드 또한 패자가 남게 되고, 최상위 노드에 **전체 승자 원소가 표시**된다.
* 최상위 노드의 값을 출력하고 제거한 다음 삭제된 값 쪽의 배열에서 다음 값을 삽입하고 **트리를 다시 구성**한다.



## 순열과 조합

### 순열(Permutation)

 서로 다른 n개의 원소들 중에서 특정 r개의 원소들을 **중복없이 순서대로 나열**하는 경우로, 원소들의 집합이 동일해도 **뽑은 순서가 다르면 서로 다른 경우의 수**로 친다. 흔히 nPr로 표현한다.

 ![Permutation](https://github.com/presentnine/Algorithm/blob/master/Algorithm6/Permutation.gif)

 특정 배열이 존재할 때, C++에서는 [next_permutation()](https://docs.microsoft.com/ko-kr/cpp/standard-library/algorithm-functions?view=vs-2019#next_permutation) 함수를 통해 다음 순열을 구할 수 있다. 

 또는 직접 구현을 하기 위해서는, 해당 배열에 대한 **재귀 호출**을 통해 순서를 바꾸는 방식으로 구현할 수 있다. 하지만 이 경우 시간 복잡도가 **O(n!)** 이다.



#### 순열의 의사 코드

```c++
permutation(A[], pos, size) //이 경우 A배열은 이미 기존 값이 채워진 상태
    if(pos == size) //Base case
        //한 순열이 완성된 상태
    else
        for i <- pos to size do
            swap(A[pos], A[i])
            permutation(A, pos+1, size)
            swap(A[pos], A[i])
```

* 특정 위치에 대하여 이후의 배열 **원소들과 교환**하고 **재귀 함수 호출**
* 이후 다**시 교환하여 원 상태로 돌려놓음**(해당 위치의 원소에 대해 다음 순서의 원소와 교환을 하기 위해)



### 조합(Combination)

 서로 다른 n개의 원소들 중에서 특정 r개의 원소들을 **중복없이 뽑는** 경우로, 원소들의 **집합이 동일하면 동일한 경우의 수**로 친다. 흔히 nCr로 표현한다.![combination](https://github.com/presentnine/Algorithm/blob/master/Algorithm6/combination.jpeg)

 조합에 대해서는 C++에서 제공해주는 함수는 존재하지 않는다.

 직접 구현을 하기 위해선, **재귀 호출**을 이용해 집합을 채워나가는 방식을 사용할 수 있다.



#### 조합의 의사 코드

```c++
combination(A[], pos, n, size, idx) //이 경우 A배열은 빈 배열을 나타냄, size는 원하는 집합의 크기
    if(pos==size) //Base case
        //한 조합이 완성된 상태
    if(n == idx) //지정된 배열의 끝에 다다른 경우 -> 집합 생성이 더 불가
        return;
    A[pos]= List[idx]
    combination(A, pos+1, n, size, idx+1)
    combination(A, pos, n, size, idx+1)
```

* 현 위치의 배열에 원소를 옮겨 담고, **두 가지의 조건**으로 재귀함수 호출
  * **다음 배열의 인덱스**에 지정된 배열의 **다음 원소**를 넘기는 경우
  * **배열의 현재 인덱스**에 지정된 배열의 **다음 원소**를 덮어 씌워버리는 경우
