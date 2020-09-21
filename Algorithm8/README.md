# 알고리즘 8(그래프 2)

## 최단경로

 네트워크에서 정점 u와 정점 v를 연결하는 경로 중에서 간선들의 **가중치 합이 최소**가 되는 경로



### Dijkstra의 최단 경로 알고리즘

 하나의 시작 정점으로부터 모든 다른 정점까지의 최단 경로를 찾는 알고리즘으로, 시작 **정점 v와의 최단 경로가 이미 발견된 집합 S를 사용**하여 새로운 정점을 S에 추가할 때마다 **경로의 값을 갱신**한다.

![Dijkstra1](C:\Users\admin\Desktop\MD 폴더\알고리즘\알고리즘 8\Dijkstra1.png)

![Dijkstra2](C:\Users\admin\Desktop\MD 폴더\알고리즘\알고리즘 8\Dijkstra2.png)

#### Dijkstra 최단 경로 의사 코드

```c++
weight[][], distance[], S[];

shortest_path(start, n
    distance[start]=0
    for i<-0 to n-1 do
        u <- choose(distance, n, S)//아직 속하지 않은 정점 중 최단 경로를 가진 정점
        S[u] = True
        for w<-0 to n-1 do
            if(!S[w])
                if(distance[u]+weight[u][w]<distance[w])
                    distance[w]=distance[u]+weight[u][w]

```

* 시작 정점 경로를 0으로 초기화
* 집합 S에 속하지 않은 정점들 중 **최단 경로를 가진 정점을 찾아**, S에 속하게 한 후 다른 전체 정점에 대하여 **경로의 값을 갱신**
* 전체 정점에 대해 **반복**



#### 특징

* 네트워크에 n개의 정점이 있다면, **O(n^2)** 의 시간 복잡도를 가진다
  * 외부 반복문을 n번 반복
  * 내부 반복문을 n번 반복



## MST(Minimum Spanning Tree)

 네트워크에 있는 모든 정점들을 **가장 적은 수의 간선과 비용으로 연결**하는 방법

#### MST 조건

* 간선의 가중치의 합이 최소여야 한다.
* 반드시 **n-1개**의 간선만 사용해야 한다.
* **사이클이 포함되어서는 안된다**.



### Kruskal 알고리즘

 그래프의 간선들을 가중치의 **오름차순으로 정렬**한 다음, **사이클을 이루지 않는** 최소 비용 간선을 선택하여 MST **집합에 추가**하는 알고리즘

* 이 때 사이클을 이루는지 아닌지 판단할 때 **Union-Find** 알고리즘을 사용한다.

![Kruskal](C:\Users\admin\Desktop\MD 폴더\알고리즘\알고리즘 8\Kruskal.png)

#### Kruskal MST 의사코드

```c++
Kruskal(Graph G)
    sort(E)
    Ecounter <- 0, k <- 0, Emst <- Null
    
    while Ecounter < n-1 do
        k <- k+1
        if(Emst U E[k] is not cycle)
            Emst <- Emst U Ek
            Ecounter <- Ecounter + 1 
            
    return Emst
```

* 간선들의 집합을 **오름차순으로 정렬**
* 최소 간선부터 올라가며 진행
* 만약 해당 간선을 포함시켜서 **사이클 생성이 되지 않는다면**, **MST 집합**에 해당 **간선을 포함**시키고 카운트 증가
* 이를 MST가 완성될 때까지 반복



#### 특징

* Kruskal 알고리즘은 대부분 **간선들을 정렬하는 시간에 좌우**된다.
* 총 간선의 개수를 e라고 할 때 **정렬 알고리즘을 효율적인 알고리즘**으로 정렬한다면 복잡도는 **O(e*log(e))** 이다.
* 간선의 개수가 적은 **희소 그래프에서 효율적**



### Prim 알고리즘

 **시작 정점**에서부터 출발하여 MST 집합을 단계적으로 확장해 나가는 알고리즘으로, 해당 집합에서 **인접한 정점** 중에서 **최저 간선으로 연결된 정점**을 선택하여 확장해 나간다.

![Kruskal](C:\Users\admin\Desktop\MD 폴더\알고리즘\알고리즘 8\Prim.png)

#### Prim MST 의사코드

```c++
Prim(Graph G, s)
    Vcounter <- 0, Vmst <- s
    
    while Vcounter < n do //n은 정점의 수
        u <- get_min_vertex(s) //현 집합과 인접한 정점 중 최저 간선을 가지는 정점 u
        if(distance[u] != INF)
            Vt <- Vt U u
            Vcounter <- Vcounter + 1
            
    return Vmst
```

* 시작 노드를 MST 집합에 넣고 시작
* 현 집합과 인접한 정점 중 **최저 간선을 가지는 정점 u를 찾아 반환**
* 해당 간선의 길이가 무한이 아니면, MST **집합에 u를 넣고** 카운트 증가
* n번 만큼 **반복**



#### 특징

* 정점의 개수가 n개라면, **O(n^2)** 의 시간 복잡도를 가진다
  * 주 반복문을 n번 반복
  * 내부 반복문을 n번 반복
* 간선의 개수가 많은 **밀집 그래프에서 효율적**

