# 알고리즘 5

#### 탐색(search)이란?

 여러 개의 자료 중에서 **원하는 자료를 찾아내는 작업**으로, 컴퓨터가 가장 많이 수행하는 작업 중의 하나

* **탐색 키(search key):** 탐색에 사용되는 필드
* 사용되는 자료 구조:  배열, 연결 리스트, 트리, 그래프 등



## 선형 탐색(순차 탐색)

 탐색 방법 중에서 가장 간단하고 직접적인 탐색 방법으로, 정렬되지 않은 배열을 **처음부터 마지막까지 하나씩** 검사하는 방법

![linearsearch](https://github.com/presentnine/Algorithm/blob/master/Algorithm5/linearsearch.png)

#### 선형 탐색의 의사 코드

```c++
linear_search(key, low, high, list[])
    for i <- low to high do
        if(list[i]==key)
            return i;
    return -1; //fail
```



* 복잡도: **O(n)**
* 평균 비교 횟수
  * 탐색 성공: (n+1)/2번 비교
  * 탐색 실패: n번 비교
* 특징
  * 매우 단순한 만큼, 데이터 수가 많아지면 **시간이 많이 소요**된다.
  * 리스트가 **정렬되어 있지 않아도 사용**할 수 있다.



## 이진 탐색(이분 탐색)

 **정렬되어 있는 배열**에서, 배열의 **중앙에 있는 값을 조사**하여 찾고자 하는 키 값이 왼쪽인지 오른쪽에 있는지를 알아내어 **탐색 범위를 반으로 줄여가는** 탐색

![binarysearch](https://github.com/presentnine/Algorithm/blob/master/Algorithm5/binarysearch.png)

#### 이진 탐색의 의사 코드

```c++
binary_search(key, low, high, list[]) //리스트가 오름차순 정렬
    while(low <= high)
        middle <- (low + high)/2
        if(key == list[middle])
            return middle
        else if(key > list[middle])
            low <- middle + 1
        else
            high <- middle - 1
    return -1; //fail
```



* 복잡도: **O(log(n))**
  * 하지만 사용하는 리스트가 정렬되어 있지 않다면, **정렬하는 데 추가적인 복잡도** 요구
* 특징
  * 사용하려는 리스트가 **정렬되어 있어야 한다.**



## 해시 탐색

 **해싱**을 사용하여 자료를 저장하고, 해당 함수를 통해 찾으려는 자료의 위치를 알아내는 탐색

* **해싱(hashing)**: 키 값에 대해 산술적 연산에 의해 테이블의 주소를 계산하여 항목에 접근
* **해시 함수(hash function)**: 탐색키를 입력받아 **해시 주소(hash address)** 생성
* **해시 테이블(hash table)**: 키 값의 연산에 의해 직접 접근이 가능한 구조
  * 이 때의 해시 테이블의 **인덱스**가 **해시 주소**



![hashsearch](https://github.com/presentnine/Algorithm/blob/master/Algorithm5/hashsearch.png)



#### 충돌이란?

* 서로 다른 탐색 키를 갖는 항목들이 같은 해시 주소를 가지는 현상
* 해결책
  * **개방주소 방법(Open Addressing)**: 충돌이 일어난 항목을 해시 테이블의 다른 위치에 저장
  * **체이닝(Chaining)**: 같은 주소를 갖는 원소들을 연결 리스트로 관리



* 복잡도: **O(1)**
  * 다만 동일한 해시 값을 같는 해시 충돌이 일어날 경우, 최대 **O(n)** 의 복잡도를 가질 수 있다.
* 해시 탐색을 구현하기 위해선 데이터를 저장하는 알고리즘, 데이터를 검색하는 **알고리즘 2가지**가 필요하다.
* 실제로는 해시테이블의 **크기가 제한**되기 때문에, 존재 가능한 **모든 키에 대해 저장 공간을 할당할 수 없다.**



## BST(Binary Search Tree)

 이진 탐색과 같은 원리에 의한 탐색 구조이나, 배열에 저장되어 삽입/삭제가 어려운 이진 탐색과 달리 삽입/삭제가 효율적인 구조

* key(왼쪽 서브 트리) <= key(루트 노드) <= key(오른쪽 서브 트리)
* 연결리스트로 구현

![binary_search_tree](https://github.com/presentnine/Algorithm/blob/master/Algorithm5/binary_search_tree.jpg)

#### 이진 탐색 트리에서의 탐색

```c++
search(t, key) //t는 트리
    if t = NULL
        return NULL
    if key = t->key
        return t
    else if k < t->key
        return search(t->left, key)
    else
        return search(t->right, key)
```

* 해당 트리가 비어있으면, NULL 반환
* 값이 맞으면, 해당 값 반환
* 현재 노드의 값이 키 값보다 **크면**, **왼쪽 서브 트리**에 대해 탐색 호출
* 현재 노드의 값이 키 값보다 **작으면**, **오른쪽 서브 트리**에 대해 탐색 호출



#### 이진 탐색 트리에서의 삽입

```c++
insert_node(t, node) //t는 트리
    p <- NULL, t <- root
    while t != NULL
        p <- t
        if node->key < p->key
            t <- p->left
        else
            t <- p->right
            
    if p = NULL //트리가 비어있는 경우
        root <- node
    else if node->key < p-> key
        p->left <- node
    else
        p->right <- node
```

* 루트 노드 부터 시작, 삽입할 노드의 키 값을 바탕으로 노드를 타고 내려간다.
* 이후 리프 노드와 최종 비교를 해 노드를 제 위치에 삽입



#### 이진 탐색 트리에서의 삭제

**3 가지 경우**

* 삭제하려는 노드가 단말 노드일 경우
  * 단말 노드의 **부모 노드를 찾아서 연결을 끊음**
* 삭제하려는 노드가 왼쪽이나 오른쪽 서브트리 중 하나만 가지고 있는 경우
  * 단말 노드의 **서브 트리를 부모 노드에 붙임**
* 삭제하려는 노드가 두 개의 서브트리 모두 가지고 있는 경우
  * 단말 노드의 **선행자 또는 후행자를 찾아 삭제할 노드 위치로 가져옴**
  * **선행자:** 노드의 **왼쪽** 서브 트리에서 **제일 큰 값**
  * **후행자:** 노드의 **오른쪽** 서브 트리에서 **제일 작은 값**



#### 성능 분석

* 이진 탐색 트리에서의 탐색, 삽입, 삭제 모두 시간 복잡도는 트리의 높이에 비례
  * 트리의 높이를 h라 했을때 복잡도는 **O(h)**
  * 최선의 경우: 이진 트리가 균형적으로 생성되어 있는 경우
    * **h = [log(n+1)]**
  * 최악의 경우: 이진 트리가 한쪽으로 치우쳐진 경우
    * **h = n**
    * 순차 탐색과 시간 복잡도가 같아진다.
