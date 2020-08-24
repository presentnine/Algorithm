# 알고리즘 4

## 합병 정렬(merge sort)

  리스트를 두 개의 균등한 크기로 **분할**하고 분할된 **부분 리스트를 정렬**, 이후 다시 합병하여 **전체 리스트를 정렬**하는 방식으로, **분할 정복** 방법을 사용한다.

![mergesort](https://github.com/presentnine/Algorithm/blob/master/Algorithm4/mergesort.gif)



#### 합병 정렬의 의사 코드

```c++
merge_sort(A[], left, right)
    if(left < right)
        mid = (left+right)/2
        merge_sort(A[], left, mid)
        merge_sort(A[], mid+1,right)
        merge(A[], left, mid, right)
```

* 현 구간의 **크기가 2 이상**인 조건
* 중간 위치 계산
* **왼쪽 부분 배열 정렬**을 위한 merge_sort 함수 **재귀 호출**
* **오른쪽 부분 배열 정렬**을 위한 merge_sort 함수 **재귀 호출**
* 정렬된 2개의 부분 배열을 합하여 **하나의 배열을 다시 정렬**

##### merge 함수의 의사 코드

```c++
merge(A[], left, mid, right)
    i<-left, j<-mid+1, k<-left, sorted[]
    while(i<=mid And j<=right)
        if(list[i]<list[j])
            sorted[k++]<-list[i++]
        else
            sorted[k++]<-list[j++]
    if(i<=mid)
        for(;i<=mid;i++)
            sorted[k++]<-list[i++]
    if(j<=right)
        for(;j<=right;j++)
            sorted[k++]<-list[j++]
```

* 배열 A 크기와 **동일한 크기의 sorted 배열 생성**
* 왼쪽 부분 배열과 오른쪽 부분 배열 간의 **요소 비교**
* sorted 배열에 **정렬된 값을 대입**
* 이후 남은 부분 배열의 요소 **일괄 대입**



#### 합병 정렬의 복잡도

* **최선, 평균, 최악**의 경우 **큰 차이 없이 O(nlog(n))의 복잡도**를 가짐
* **특징**
  * 배열 간의 **데이터 이동이 빈번**하여 레코드의 크기가 큰 경우에는 매우 큰 시간이 걸림
    * 이는 레코드를 **연결 리스트로 구현하면 완화**
  * **안정적**이며 데이터의 **초기 분산 순서**에 영향을 **덜 받는 편**



## 힙 정렬(heap sort)

 조건에 맞게 **최대 힙 트리**, **최소 힙 트리**를 구성하여 정렬하는 방법, 기존 요소들의 배열 요소들을 **순서대로 힙 트리에 삽입**을 한 후, 루트를 계속 삭제하여 배열에 옮겨 적는 방식

![heapsort](https://github.com/presentnine/Algorithm/blob/master/Algorithm4/heapsort.png)

#### 힙 정렬의 의사 코드(내림차순 정렬을 위한 최대 힙)

```c++
heap_sort(A[], n)
    heap[]
    for i<-0 to n-1 do
        insert_max_heap(heap[], A[i])
    for i<-0 to n-1 do
        A[i]<-delete_max_heap(heap[])
```

* 힙 트리(배열) 생성
* 배열 요소를 순차적으로 최대 힙 트리에 삽입
* 이후 순서대로 힙에서 삭제해 배열에 대입

##### insert_max_heap 함수의 의사 코드

```c++
insert_max_heap(heap[], key) //key==A[i]
    heap_size <- heap_size + 1
    i <- heap_size
    heap[i] <- key
    
    while i != 1 And heap[i] > heap[parent(i)] do
        swap(heap[i],heap[parent(i)])
        i <- parent(i)
```

* 새로운 요소를 삽입하기 위해 먼저 힙의 마지막 노드에 이어서 삽입
* 힙의 성질이 만족되지 못하면 **부모 노드와 비교, 교환**
* 이후 **힙의 성질을 만족**하거나 **루트** 노드에 **다다를 때까지 반복**

##### delete_max_heap 함수의 의사 코드

```c++
delete_max_heap(heap[])
    item <- heap[1]
    temp <- heap[heap_size--]
    parent <- 1, child <- 2
    
    while(child <= heap_size)
        if(child < heap_size And heap[child] < heap[child+1])
            child++
        if(temp >= heap[child])
            break
            
        heap[parent] <- heap[child]
        parent <- child
        child <- child*2
    heap[parent]<-temp
    return item
```

* 루트 노드를 미리 복사, 힙의 마지막 원소를 temp로 복사
* 자식 노드 중 **더 큰 자식 노드를 찾고**, 이를 **temp 값과 비교**
* temp 값이  크거나 같으면 거기서 반복을 멈춤
* 그렇지 않다면 child값을 트리 위로 올리고 **parent, child 값을 높여** 다음 레벨의 힙에 대해 **탐색을 반복**
* 최종 **parent 위치에 temp값 삽입**
* item값 **반환**



#### 힙 정렬의 복잡도

* 삽입과 삭제 함수 모두 **최악의 경우: O(log(n))**
  * 트리의 높이만큼 비교 연산, 이동 연산을 해야 한다
* 이를 정렬할 **배열의 크기 n만큼 반복**하기 때문에 복잡도는 **O(nlog(n))**
* 특징
  * 전체의 정렬이 아니라 **가장 큰 값 몇개만 필요할 때** 최적이다



## 셸 정렬(shell sort)

 삽입 정렬이 어느 정도 정렬된 상태에서 빠른 것을 이용한 정렬. 이웃한 위치로만 교환하는 **삽입 정렬을 보완**하여 전체 리스트를 **일정 간격(gap)의 부분 리스트로 나눠** 해당 리스트에 대해 **삽입 정렬을 진행**

![shellsortA](https://github.com/presentnine/Algorithm/blob/master/Algorithm4/shellsortA.png)

![shellsortA](https://github.com/presentnine/Algorithm/blob/master/Algorithm4/shellsortB.png)



#### 셸 정렬의 의사 코드

```c++
shell_sort(A[], n)
    for(gap = n/2; gap > 0; gap = gap/2)
        if gap%2 == 0
            gap++
        for(i = 0; i < gap; i++)
            inc_insertion_sort(A[], i, n-1, gap)
```

* 전체 리스트를 **일정 간격(gap)** 의 **부분 리스트**로 분할
* 해당 부분 리스트에 대해 **삽입 정렬 수행**
* 이를 간격의 크기를 **1까지** 계속 줄여가며 **반복**

##### inc_insertion_sort 함수의 의사 코드

```c++
inc_insertion_sort(A[], first, last, gap)
    for(i = first+gap; i<=last; i=i+gap)
        key <- A[i]
        for(j = i-gap; j >= first And A[j] > key; j = j-gap)
            A[j+gap] <- A[j]
        A[j+gap] <- key
```

* **일정 간격(gap)** 의 부분 리스트에 대하여 삽입 정렬 진행



#### 셸 정렬의 복잡도

* **최악**의 경우 **O(n^2)**
* **평균의 경우 O(n^1.5)**
* **특징**
  * 삽입 정렬보다 **적은 위치 교환**으로 맞는 자리를 찾을 **가능성이 증대**
  * 점진적으로 **정렬된 상태**가 되므로 삽입 정렬 **속도가 증가**
