# 알고리즘 3

#### 정렬(Sort)이란?

 물건을 크기순으로 오름/내림차순으로 **나열**하는 것으로, 가장 기본적이고 중요한 알고리즘 중 하나

#### 정렬의 대상

 일반적으로 정렬시켜야 될 대상은 **필드(field)** 들로 구성된 **레코드(record)** 로, **키(Key) 필드**를 통해 레코드들을 구별할 수 있다.

#### 최적의 알고리즘?

 모든 경우에 최적인 정렬 알고리즘이 **존재하지 않기** 때문에, 레코드 개수, 레코드 크기, Key의 특성, 내부 정렬/외부 정렬 등을 가지고 판단하여 **가장 적합한 정렬 방법** 을 찾아 사용해야 한다. 

* **내부 정렬(internal sorting):** 모든 데이터가 주기억장치에 저장된 상태에서 정렬
* **외부 정렬(external sorting):** 외부 기억 장치에 대부분의 데이터가 있고 일부만 주기억장치에 저장된 상태에서 정렬

#### 정렬 알고리즘의 안정성(stability)

 동일한 키 값을 갖는 레코드들의 **상대적인 위치** 가 정렬 후에도 바뀌지 않음을 나타낸다.



## 삽입 정렬(insertion sort)

  정렬되어 있는 부분에 __새로운 레코드를 올바른 위치에 삽입__하는 과정을 반복하는 정렬

![insertion sort](https://github.com/presentnine/Algorithm/blob/master/Algorithm3/insertion%20sort.gif)



#### 삽입 정렬의 의사 코드

```c++
insertion_sort(A[], n)
    for i <- 1 to n-1 do
        key <- A[i];
        j <- i-1;
        while j >= 0 and A[j] > key do
            A[j+1] <- A[j];
            j <- j-1;
        A[j+1] <- key
```

* 현재 i 번째 정수인 A[i]를 key 변수로 복사, i - 1 번째 부터 역순으로 조사를 수행
* j 값이 0이상이고, key 값보다 A[j]의 값이 더 크다면 A[j]의 값을 A[j+1]로 이동
* j를 하나씩 감소하며 **반복**
* 조건을 만족하지 못하면 A[j]의 값이 key 값보다 작기 때문에, **A[j+1] 위치에 key 값을 넣음**
* 1부터 n-1 까지 반복

[여러 언어 버전의 코드]([https://ko.wikipedia.org/wiki/%EC%82%BD%EC%9E%85_%EC%A0%95%EB%A0%AC](https://ko.wikipedia.org/wiki/삽입_정렬))



#### 삽입 정렬의 복잡도

* **최선의 경우 O(n):** 이미 정렬되어 있는 경우 
  * 비교: n-1 번
* **최악의 경우 O(n^2):** 역순으로 정렬되어 있는 경우
  * 비교: n(n-1)/2 번
  * 이동: n(n-1)/2 번
* **평균의 경우 O(n^2)**
* **특징**
  * **많은 이동**을 필요로 함
  * **안정된** 정렬 방법
  * **대부분 정렬되어 있으면 매우 효율적**



## 버블 정렬(bubble sort)

 **인접한 2개의 레코드를 비교하여 순서대로 되어 있지 않으면 교환하는 방식**을 사용하는 정렬로, 이를 왼쪽에서부터 오른쪽 끝까지 반복하여 **가장 큰 레코드를 오른쪽 끝으로 보내고** 이를 제외한 **남은 리스트에 대하 해당 과정을 반복한다.**

![bubble sort](https://github.com/presentnine/Algorithm/blob/master/Algorithm3/bubble%20sort.png)



#### 버블 정렬의 의사 코드

```c++
bubble_sort(A[], n)
    for i <- n-1 to 1 do
        for j <- 0 to i-1 do
            if(A[j] > A[j+1])
                SWAP(A[j], A[j+1]);
```

* A[j]와 A[j+1]이 조건에 맞지 않으면 교체
* 이를 j가 0부터 i-1번째 까지 반복
* 해당 i 값을 n-1부터 1까지 값을 내려가며 반복

[여러 언어 버전의 코드]([https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC](https://ko.wikipedia.org/wiki/거품_정렬))



#### 버블 정렬의 복잡도

* **최선, 최악, 평균**의 경우 모두 일정하게 **O(n^2)**
* **최선의 경우** O(n^2): 이미 정렬되어 있는 경우 
  * 비교: n(n-1)/2 번
* **최악의 경우** O(n^2): 역순으로 정렬되어 있는 경우
  * 비교: n(n-1)/2 번
  * 이동: n(n-1)/2 번
* **특징**
  * 상당히 **많은 레코드들의 이동을 요구**함(이동 연산은 비교 연산보다 **더 많은 시간이 소요**)



## 퀵 정렬(quick sort)

 **분할 정복** 기법을 사용하는 정렬로, 피벗(기준)을 정하여 피벗을 기준으로 리스트를 **2개의 부분 리스트**로 **비균등 분할**하고, 각 부분 리스트를 다시 퀵 정렬한다.

![quicksort](https://github.com/presentnine/Algorithm/blob/master/Algorithm3/quicksort.gif)

 이 때 피벗을 결정하는 기준은 다양하다 (리스트의 맨 앞, 맨 뒤 등)



#### 퀵 정렬의 의사 코드

```c++
quick_sort(A[], left, right)
    if(left< right)
        q <- partition(A[], left, right);
        quick_sort(A[], left, q-1);
        quick_sort(A[], q+1, right);
```

* partition 함수를 통해 **피벗을 구함** (정확히는 배열 A 속 **피벗의 위치**가 반환)
* left에서 부터 q-1까지를 퀵정렬 **(분할 정복)**
* q+1에서부터 right까지를 퀵정렬 **(분할 정복)**

##### partition 함수의 의사 코드 (피벗을 리스트 맨 앞으로 설정)

```c++
partition(A[], left, right)
    low <- left, high <- right+1, pivot <- A[left]
    do
        do
            low++
        while(low <= right And A[low] < pivot)
        do
            high--
        while(high >= left And A[high] > pivot)
        if(low < high)
            swap(A[low], A[high])
     while(low < high)
         
     swap(A[left], A[high])
     return high;
```

* 리스트 내에서 **피벗보다 큰 값**이 나올 때까지 low 위치 증가
* 리스트 내에서 **피벗보다 작은 값**이 나올 때까지 high 위치 감소
* low가 high보다 낮은 위치라면 **둘의 값 교환**
* low가 high보다 낮은 위치라면 위의 과정 반복
* 마지막으로 left와 high 위치의 값 교환 (**피벗이 순서에 맞는 위치에 들어감**)
* high 값 반환 **(피벗 위치)**

![quick-sort one step](https://github.com/presentnine/Algorithm/blob/master/Algorithm3/quick-sort%20one%20step.jpg)

#### 퀵 정렬의 복잡도

* **최선**의 경우 **O(nlog(n)):** 거의 균등한 리스트로 분할 되는 경우
* **최악**의 경우 **O(n^2):** 극도로 불균등한 리스트로 분할 되는 경우
  * 이런 경우 **중간값**을 피벗으로 선택하여 불균등을 어느 정도 완화시킬 수 있다.
* **평균의 경우 O(nlog(n))**
* **특징**
  * 속도가 빠르며, 추가적인 메모리 공간을 요구하지 않는다.
  * 오히려 정렬된 리스트에서 수행시간이 더 많이 걸린다.
