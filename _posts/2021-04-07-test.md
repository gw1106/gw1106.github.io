---
layout: single
title:  "Dijkstra!"
date:   2021-04-05 22:00:00 +0900
categories: jekyll update
---
##### Greedy2

#과제: 다익스트라 알고리즘 자바로 구현하기



### 1. 다익스트라 = ?
![Dijkstra](https://upload.wikimedia.org/wikipedia/commons/5/57/Dijkstra_Animation.gif)

데이크스트라 알고리즘(영어: Dijkstra algorithm) 또는 다익스트라 알고리즘은 도로 교통망 같은 곳에서 나타날 수 있는 그래프에서 꼭짓점 간의 최단 경로를 찾는 알고리즘이다. 
이 알고리즘은 변형이 많지만 우리가 구현한 알고리즘은 한 꼭짓점을 "소스" 꼭짓점으로 고정하고 그래프의 다른 모든 꼭짓점까지의 최단경로를 찾는 알고리즘으로 최단 경로 트리를 만드는 것이다.


### 2. 알고리즘

1. 하나의 시작 정점을 설정하고 이를 기준으로 각 최소 비용을 배열에 저장합니다.
 바로 갈 수 없는 곳의 값은 무한대로 설정합니다.
2. 방문하지 않은 점 중 가장 비용이 적은 정점을 선택합니다.
3. 해당정점을 거쳐 특정한 정점으로 가는 경우를 고려하여 최소비용을 변경합니다.
4. 위의 과정을 반복해 최단 경로를 탐색합니다.


### 3. 자바로 구현(소스코드 전체)
```java
package greedy.mst;


import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.PriorityQueue;

public class dijkstra {
    static int INF = 10000000; // 전역 변수 선언


    public static int[] solve(int[][] graph, int start){

        int [] dist = new int [graph.length];
        Arrays.fill(dist, INF);

        dist[start] = 0;  //   [INF], [0], [INF], [INF] ,[INF], [INF], [INF]
        PriorityQueue<Node> queue = new PriorityQueue<Node>();
        queue.add(new Node(0, start));

        while (!queue.isEmpty()){
            Node edge = queue.poll();  // 가장 적은놈을 빼서 edge에 저장
            int here = edge.loca;

            for (int i = 0; i<graph.length-1; i++){
                if (graph[here][i]!= INF){   // 이동 할 수 있는 노드라면
                    int there = i+1 ;  // 0번째부터 이기에 1 더해준다 (0은 인위적인 인덱스라)
                    int length = graph[here][i];
                    int next_dist = dist[here]+length;

                    if (next_dist<dist[there]){
                        dist[there] = next_dist;
                        queue.add(new Node(next_dist,there));
                    }
                }
            }
        }
        return dist;
    }

    public static class Node implements Comparable<Node> {
        int dist;
        int loca;

        public Node(int dist, int loca) {
            this.dist = dist;
            this.loca = loca;
        }


        @Override
        public int compareTo(Node o) {
            return this.dist - o.dist;
        }
    }

    public static void main(String[] args) {

        int [][] graph = {{  },  // 노드 번호로 직관적으로 참조하기위해 0번째는 그냥 인위적으로 생성
                {INF , 2, 3, 6, INF,INF},
                {INF,INF,INF,INF,INF,4},
                {INF,INF,INF,1,INF,INF},
                {INF,INF,INF,INF,INF,INF},
                {INF,INF,INF,INF,INF,INF},
                {INF,INF,INF,INF,INF,INF}};

        int [] dist = solve(graph,1);
        for (int i = 1; i<dist.length; i++){   // 0번째는 필요없는 인위적인 index라 1부터 참조
            if (dist[i] != INF){
                System.out.printf("1번 노드에서 %d번 노드까지의 최단거리 : %d \n",i,dist[i]);
            }
            else {
                System.out.printf("1번 노드에서 %d번 노드까지의 최단거리 : INF(갈 수 없다) \n",i);
            }
        }
    }
}
```


### 4. 소스코드 설명

#### (1)그래프

![그래프 생성](https://user-images.githubusercontent.com/81409594/113568908-6d474400-964c-11eb-9a3c-1bfcd52d7146.png)

메인 함수에서 
그래프를 생성한다 
총 노드는 6개이지만 배열은 0번째부터 시작이므로 알아보기 쉽게 하기 위해 0번째에 비어있는 index를
추가하였다.

참고로 INF는 갈 수 없는 노드까지의 거리이다.
그래서 그래프는
1번째 노드가 2번째로 2 만큼 3번째로 3만큼 4번째로 6만큼
2번째 노드가 6번째로 4만큼
3번째 노드가 4번째로 1만큼 이동하는 그래프이다

최단 거리를 구하는 게 목표이기에 loop를 돌게 하려면 그래프 배열을 수정하면 그만이다. 아래사진은 위 코드를 구현한 것이다. 

![스크린샷(25)](https://user-images.githubusercontent.com/81409594/113567089-e5ac0600-9648-11eb-85f2-1dd5f6250954.png)


#### (2)우선순위 큐


![우선순위 큐 자료형 만들기](https://user-images.githubusercontent.com/80373033/113569285-302f8180-964d-11eb-9c71-4cd1e4293b87.png)

우선순위 큐를 선언할 때의 자료형을 만들어주는 코드이다
이름은 Node로 하고 안에 dist와 loca라는 변수를 만들어준다
그러고는 Node.dist 와 Node.loca로 각각의 값이 참조되게도 해준다
마지막 코드는 우선순위 큐에서의 가장 앞으로 놓을 값을 선정할 때의
기준값을 정하는 코드이다
최단 거리를 구하는 알고리즘이기에 당연히 dist를 기준값으로 하였다. 


아래 사진은 큐의 배열이 변 과정이다 
![스크린샷(29)](https://user-images.githubusercontent.com/81409594/113567172-0ffdc380-9649-11eb-908b-b82f17a2ed20.png)

![스크린샷(30)](https://user-images.githubusercontent.com/81409594/113567204-1f7d0c80-9649-11eb-8545-84d58029d817.png)

![스크린샷(31)](https://user-images.githubusercontent.com/81409594/113567218-26a41a80-9649-11eb-9f9f-fc19e7ce7727.png)




#### (3) 함수설명 1/2
![화면 캡처 2021-04-04 161719](https://user-images.githubusercontent.com/80373033/113569402-6c62e200-964d-11eb-918a-46a86d5424ef.png)

solve 함수에서 그래프와 시작 노드를 가져온다.
그러고는 dist라는 새로운 최단 거리를 저장 할 배열을 주어진 그래프의 크기만큼 가져와서 INF로 채워놓는다.
이제 dist배열의 start번째  index는 0으로 둔다. 왜냐면 본인의 노드에서 본인의 노드까지의 거리는 0이기 때문에
그러고는 우선순위 큐를 생성해서 0과 시작 노드를 넣어준다.


#### (4) 함수설명 2/2
![image](https://user-images.githubusercontent.com/81409594/113569613-c19ef380-964d-11eb-9720-bdf6e4c7bc93.png)

우선순위 큐가 비워질 때까지 반복한다.
그러고 poll 함수를 사용한다 poll은 우선순위 큐에서 사용하면 우선순위로 정렬된 큐에서 가장 
왼쪽(맨 앞) 값이 반환된다
그렇기에 edge에 반환하고 edge의 위치 값이 here에 저장.

이제 반복문을 돌려주는데 만약
그래프에서 이동 할 수 있는 노드여야만 밑에 코드까지 진행된다
이동할 수 있다면 이동하는 노드의 위치를 there에 저장하고 there까지의 거리를 length에 저장한다
그러고는 기존이동 거리와 there까지의 이동 거리인 length를 더해서 next_dist에 저장한다

만약 next_dist가 기존의 이동 거리보다 짧다면 dist[there]을 next_dist로 새로 고침 해준다.
그러고는 다시 그 노드를 우선순위 큐에 삽입해준다

우선순위 큐가 비워질 때까지 반복문이 실행하면서 마지막으로는 최단 거리가 표현된 dist가 
반환된다


#### (5) 함수 결과반환, 출력
![image](https://user-images.githubusercontent.com/81409594/113569743-f317bf00-964d-11eb-9934-d0de2780ae64.png)

solve함수에 조금 전 만든 graph와 시작 노드값을 건네주고
이 결괏값을 dist라는 새로운 배열에 넣어준다
그러면 dist라는 배열에 시작 노드로부터 각각의 노드까지의 최단 거리가 저장된다.

이제 출력하기 위해 반복문을 도는데 1번부터 돌려야 한다. 왜냐면 dist[0]에는 보기 쉽게 하기 위해 
인위적으로 만든 값이 있기에 출력할 필요가 없다.
그래서 dist[1]부터 각 index에 저장된 값이 INF가 아니라면 최단 거리를 출력하고
INF라면 INF로 출력한다.















### 5. 결과
![화면 캡처 2021-04-06 151837](https://user-images.githubusercontent.com/80373033/113667257-79cea980-96eb-11eb-9e51-6b2ea660f767.png)




 

### 참조
[https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%81%AC%EC%8A%A4%ED%8A%B8%EB%9D%BC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98#:~:text=%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B3%BC%ED%95%99%20%EC%97%90%EC%84%9C%2C%20%EB%8D%B0%EC%9D%B4%ED%81%AC%EC%8A%A4%ED%8A%B8%EB%9D%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98%20%28%20%EC%98%81%EC%96%B4%3A%20Dijkstra,%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98%EC%9C%BC%EB%A1%9C%20%EC%B5%9C%EB%8B%A8%20%EA%B2%BD%EB%A1%9C%20%ED%8A%B8%EB%A6%AC%20%EB%A5%BC%20%EB%A7%8C%EB%93%9C%EB%8A%94%20%EA%B2%83%EC%9D%B4%EB%8B%A4.%20](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%81%AC%EC%8A%A4%ED%8A%B8%EB%9D%BC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98#:~:text=%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B3%BC%ED%95%99%20%EC%97%90%EC%84%9C%2C%20%EB%8D%B0%EC%9D%B4%ED%81%AC%EC%8A%A4%ED%8A%B8%EB%9D%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98%20%28%20%EC%98%81%EC%96%B4%3A%20Dijkstra,%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98%EC%9C%BC%EB%A1%9C%20%EC%B5%9C%EB%8B%A8%20%EA%B2%BD%EB%A1%9C%20%ED%8A%B8%EB%A6%AC%20%EB%A5%BC%20%EB%A7%8C%EB%93%9C%EB%8A%94%20%EA%B2%83%EC%9D%B4%EB%8B%A4.%20)
[https://it-garden.tistory.com/241](https://it-garden.tistory.com/241)






