# H_Group

---
layout: single
title:  "Closest pair of points!"
date:   2021-03-30 22:00:00 +0900
categories: jekyll update
---

#**_과제: 분할-정복 방법으로 최근접 점의 쌍 찾기_**


###1. _최근접 점쌍 문제_
![Solution](https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Closest_pair_of_points.svg/225px-Closest_pair_of_points.svg.png)

최근접 점쌍 문제 (closest pair problem)는 계산기하학의 문제로서, 2차원 평면상에  n 개의 점이 주어졌을 때, 사이의 거리가 가장 짧은 두 점을 찾아내는 문제이다.
가능한 모든 점쌍들의 거리를 비교해 최솟값을 찾는 알고리즘은 O(n2)의 시간을 요구한다. 하지만 유클리드 공간이나 정해진 d의 차원을 가지는 Lp 공간에서는 O(n log n)의 시간에 해결 될 수 있다.


###2. _분할-정복 방법_으로 최근접 점에 쌍 문제 풀기
![DnC](https://dudri63.github.io/image/algo8-2.png)

n개의 점을 1/2로 분할하여 각각의 부분문제에서 최근점 점의 쌍을 찾고, 2개의 부분해 중에서 가장 짧은 거리를 가진 점의 쌍을 찾는다.
하지만 위 그림처럼 중간에 있는 점들 때문에 왼쪽 부분문제, 오른쪽 부분문제 가운데 부분을 모두 생각해야 한다. 따라서 단순히 2개로 
분할한 부분문제에서 더 짧은 거리의 점의 쌍이 전체 문제에서 최근접 점의 쌍이라고 할 수 없는 것이다.


###3. 알고리즘
![Algorithm](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFrfzz%2FbtqJDFZIbu7%2FNQ50TWJHLCktdGOc5ky9M0%2Fimg.png)



###4. 자바로 구현하기
{% highlight ruby %}
import java.util.*;

public class Solution {

    static int[][] position;

    public static void main(String[] args) {
        // TODO Auto-generated method stub

        Scanner sc = new Scanner(System.in);

        int N =sc.nextInt(); // 점의 개수

        position = new int[N][2]; // 각 점의 위치

        // 위치 입력
        for(int i=0; i<N; i++){
            position[i][0]=sc.nextInt(); // x값 위치
            position[i][1]=sc.nextInt(); // y값 위치
        }

        // x 중앙값 구하기
        int[] xArray = new int [N];
        for(int i=0; i<N; i++){
            xArray[i]=position[i][0];
        }
        double xMed = Xmedian(xArray);

        // 평면 2분할
        int firstNod=-1;
        int secondNod=-1;
        ArrayList<Integer> Divide1 = new ArrayList<Integer>();
        ArrayList<Integer> Divide2 = new ArrayList<Integer>();

        for(int i=0; i<N; i++){
            if(position[i][0]<=xMed){
                Divide1.add(i);
            }
            else{
                Divide2.add(i);
            }
        }

        // 각 분할에서 최소 값 선정
        double min1=1000;
        int node11 = -1;
        int node12 = -1;
        for(int i=0; i<Divide1.size()-1;i++){
            for(int j=i+1; j<Divide1.size();j++){
                int a = Divide1.get(i);
                int b = Divide1.get(j);
                double tmp = calDist(a,b);
                if(tmp<min1) min1=tmp; node11=a; node12=b;
            }
        }

        // 각 분할에서 최소 값 선정
        double min2=1000;
        int node21 = -1;
        int node22 = -1;
        for(int i=0; i<Divide2.size()-1;i++){
            for(int j=i+1; j<Divide2.size();j++){
                int a = Divide2.get(i);
                int b = Divide2.get(j);
                double tmp = calDist(a,b);
                if(tmp<min2) min1=tmp; node21=a; node22=b;
            }
        }

        // 두 분할 중 더 최소값을 선정
        double falsemin=0;
        if(min1<=min2) {
            falsemin=min1; firstNod=node11; secondNod=node12;
        }
        else {
            falsemin=min2; firstNod=node21; secondNod=node22;
        }

        // 중앙 부분 산정
        int medianXmin = (int) (xMed-falsemin)-1;
        int medianXmax = (int) (xMed+falsemin)+1;
        ArrayList<Integer> DivideMed = new ArrayList<Integer>();
        for(int i=0;i<N;i++){
            if((position[i][0]>medianXmin)&&(position[i][0]<medianXmax)){
                DivideMed.add(i);
            }
        }

        // 중앙 부분의 최소값 산정
        double realmin=falsemin;
        for(int i=0; i<DivideMed.size()-1;i++){
            for(int j=i+1; j<DivideMed.size();j++){
                int a = DivideMed.get(i);
                int b = DivideMed.get(j);
                double tmp = calDist(a,b);
                if(tmp<realmin) {
                    realmin=tmp; firstNod=a; secondNod=b;
                }
            }
        }

        System.out.println(realmin+" "+firstNod+" "+secondNod);

    }

    // 두점 사이의 거리를 구하는 공식
    public static double calDist(int a, int b){

        return Math.sqrt(Math.pow(position[a][0]-position[b][0],2)+Math.pow(position[a][1]-position[b][1],2));

    }

    // x 중앙값 구하기
    public static double Xmedian(int[] xArray){

        Arrays.sort(xArray);
        double answer=0;

        int size = xArray.length;
        // x 값이 짝수라면
        if(size % 2 == 0){
            int m = size / 2;
            int n = (size / 2) - 1;
            answer = (double)(xArray[m]+xArray[n])/2;
        }
        else{
            int m = size/2;
            answer = xArray[m];
        }

        return answer;

    }

}

{% endhighlight %}
