# Strassen Matrix Algorithm




## 1. 슈트라센 알고리즘이란?

선형대수학에서 슈트라센 알고리즘은 독일의 수학자 폴커 슈트라센(Volker Strassen)이 1969년에 개발한 행렬 곱셈 알고리즘이다. 
정의에 따라 n×n 크기의 두 행렬을 곱하면 O(n3)의 시간이 소요되지만 이 알고리즘은 대략 O(n2.807)의 시간이 소요된다.

![https://lh3.googleusercontent.com/proxy/rp2TidyXYjY8-AkUFv1jGdk6apHE5R3faccC6G00J9GYw2UhwcDwhufuD0aWKtwJ9ZskL31YMNcCZFklfKJE2Pub3p9WsWoF6TI3MAiKOfAU-hty80Ei2GLhGxJ6jT4tKxyHvCm9qRu9jCrVf7G1RFSAVvP6cg02_G3UjY9YHS8SRYsUUGr6b4JibphGsB11_YlyUFgUa0i9](https://lh3.googleusercontent.com/proxy/rp2TidyXYjY8-AkUFv1jGdk6apHE5R3faccC6G00J9GYw2UhwcDwhufuD0aWKtwJ9ZskL31YMNcCZFklfKJE2Pub3p9WsWoF6TI3MAiKOfAU-hty80Ei2GLhGxJ6jT4tKxyHvCm9qRu9jCrVf7G1RFSAVvP6cg02_G3UjY9YHS8SRYsUUGr6b4JibphGsB11_YlyUFgUa0i9)

A, B, C를 같은 크기의 정사각행렬 네 개로 나눈다.
이 과정에서는 필요한 연산의 수가 줄어 들지 않는다. 여전히 Ci,j 행렬을 계산하려면 여덟 번의 곱셈과 네 번의 덧셈이 필요하다.

이제 다음과 같은 행렬을 정의한다.

![https://t1.daumcdn.net/cfile/tistory/2413793851BE771502](https://t1.daumcdn.net/cfile/tistory/2413793851BE771502)

이 Mk 행렬들은 Ci,j 행렬을 표현하는 데 쓰이는데, 이 행렬들을 계산하는 데는 일곱 번의 곱셈(각 변수마다 한 번씩)과 10번의 덧셈이 필요하다. 
이제 Ci,j 행렬은 다음과 같이 표현할 수 있다.

이 과정에서는 곱셈이 사용되지 않기 때문에, 전체 곱셈을 일곱 번의 곱셈과 18번의 덧셈으로 처리할 수 있다. 큰 행렬에 대해서는 행렬의 곱셈이 덧셈보다 더 많은 시간을 필요로 하기 때문에 덧셈을 더 하는 대신 곱셈을 덜 하는 것이 전체적으로 더 효율적이다.
실제로는 n이 작을 경우 정의에 따라 행렬 곱셈을 하는 경우가 빠를 수도 있기 때문에, 보통 작은 n에 대해서는 일반적인 방법으로 곱셈을 하도록 구현한다.



## 2. 알고리즘
![https://slidesplayer.org/slide/16397992/95/images/10/%EC%89%AC%ED%8A%B8%EB%9D%BC%EC%8E%88%28Strassen%29+%EB%B0%A9%EB%B2%95+%E2%80%93+nxn+%ED%96%89%EB%A0%AC+%283%2F3%29.jpg](https://slidesplayer.org/slide/16397992/95/images/10/%EC%89%AC%ED%8A%B8%EB%9D%BC%EC%8E%88%28Strassen%29+%EB%B0%A9%EB%B2%95+%E2%80%93+nxn+%ED%96%89%EB%A0%AC+%283%2F3%29.jpg)



## 3. 구현하기

```java
package dnq;

import java.util.Scanner;

public class Strassen
{
    public int[][] multiply(int[][] A, int[][] B)        			// 두 개의 nxn 정사각행렬의 곱을 계산하는 메소드 생성
    {
        int n = A.length;                                			// 2차원 배열의 행렬의 길이를 n에 저장( nxn 정사각행렬을 만드는데 필요)
        int[][] R = new int[n][n];                       			// 슈트라센 알고리즘을 적용하고 난 후의 결과값을 저장할 Result행렬 생성

        if (n < 2)                                        			// 임계차원 보다 작으면 기존 메트릭스 곱으로 풀이
            R[0][0] = A[0][0] * B[0][0];
        else
        {
            int[][] A11 = new int[n/2][n/2];             		// 첫번째 행렬에서 소행렬들의 크기를 정의해줌
            int[][] A12 = new int[n/2][n/2];
            int[][] A21 = new int[n/2][n/2];
            int[][] A22 = new int[n/2][n/2];

            int[][] B11 = new int[n/2][n/2];             		// 두번째 행렬에서 소행렬들의 크기를 정의해줌
            int[][] B12 = new int[n/2][n/2];
            int[][] B21 = new int[n/2][n/2];
            int[][] B22 = new int[n/2][n/2];

                                                               		// 첫번째 행렬을 4개의 소행렬로 나눈다
            divideMatrics(A, A11, 0 , 0);
            divideMatrics(A, A12, 0 , n/2);
            divideMatrics(A, A21, n/2, 0);
            divideMatrics(A, A22, n/2, n/2);

                                                             			 // 두번째 행렬을 4개의 소행렬로 나눈다
            divideMatrics(B, B11, 0 , 0);
            divideMatrics(B, B12, 0 , n/2);
            divideMatrics(B, B21, n/2, 0);
            divideMatrics(B, B22, n/2, n/2);


            //슈트라센 알고리즘에서 사용하는 A와 B로 이루어진 새로은 행렬 M을 다음과 같이 적용시킨다
         
            int [][] M1 = multiply(add(A11, A22), add(B11, B22));         //  M1 = (A11 + A22)(B11 + B22)
            int [][] M2 = multiply(add(A21, A22), B11);                      //  M2 = (A21 + A22) B11
            int [][] M3 = multiply(A11, sub(B12, B22));                      //  M3 = A11 (B12 - B22)
            int [][] M4 = multiply(A22, sub(B21, B11));                      //  M4 = A22 (B21 - B11)
            int [][] M5 = multiply(add(A11, A12), B22);                      //  M5 = (A11 + A12) B22
            int [][] M6 = multiply(sub(A21, A11), add(B11, B12));         //  M6 = (A21 - A11) (B11 + B12)
            int [][] M7 = multiply(sub(A12, A22), add(B21, B22));         //  M7 = (A12 - A22) (B21 + B22)


            // AxB=C인 행렬 C를 행렬 M으로 다음과 같이 표현한다
           
            int [][] C11 = add(sub(add(M1, M4), M5), M7);    		// C11 = M1 + M4 - M5 + M7
            int [][] C12 = add(M3, M5);                            		// C12 = M3 + M5
            int [][] C21 = add(M2, M4);                            		// C21 = M2 + M4
            int [][] C22 = add(sub(add(M1, M3), M2), M6);   		//  C22 = M1 - M2 + M3 + M6

            // 각 AxB의 연산이 끝난 소행렬들을 결과를 나타내는 행렬 R행렬로 합쳐준다.
            put(C11, R, 0 , 0);
            put(C12, R, 0 , n/2);
            put(C21, R, n/2, 0);
            put(C22, R, n/2, n/2);
        }
        return R;                                             		 //결과값을 반환
    }

    // 두 행렬을 더해주는 메소드
    public int[][] add(int[][] A, int[][] B)
    {
        int n = A.length;
        int[][] C = new int[n][n];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                C[i][j] = A[i][j] + B[i][j];
        return C;
    }

    // 두행렬을 빼주는 메소드
    public int[][] sub(int[][] A, int[][] B)
    {
        int n = A.length;
        int[][] C = new int[n][n];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                C[i][j] = A[i][j] - B[i][j];
        return C;
    }

    // 행렬을 똑같은 크기로 4개의 소행렬로 나누어 준다.
    public void divideMatrics(int[][] P, int[][] C, int iB, int jB)
    {
        for(int i1 = 0, i2 = iB; i1 < C.length; i1++, i2++)
            for(int j1 = 0, j2 = jB; j1 < C.length; j1++, j2++)
                C[i1][j1] = P[i2][j2];
    }

    // 각 소행렬들의 연산이 끝난 후 그 값들을 하나의 행렬로 다시 합춰준다( divideMatrics메소드와 반대 역할)
    public void put(int[][] C, int[][] P, int iB, int jB)
    {
        for(int i1 = 0, i2 = iB; i1 < C.length; i1++, i2++)
            for(int j1 = 0, j2 = jB; j1 < C.length; j1++, j2++)
                P[i2][j2] = C[i1][j1];
    }

    // 슈트라센 알고리즘을 실행시킬 메인 메소드
    public static void main (String[] args)
    {
        Scanner scan = new Scanner(System.in);

        
        Strassen s = new Strassen();

        System.out.println("행렬크기를 입력하세요 (n) :");
        int N = scan.nextInt();

        System.out.println("첫 번째 행렬의 값들을 입력하세요");        // 첫 번째 행렬 A에 대한 값들을 입력받음
        int[][] A = new int[N][N];
        for (int i = 0; i < N; i++)
            for (int j = 0; j < N; j++)
                A[i][j] = scan.nextInt();

        System.out.println("두 번째 행렬의 값들을 입력하세요1");       // 두 번째 행렬 B에 대한 값들을 입력받음
        int[][] B = new int[N][N];
        for (int i = 0; i < N; i++)
            for (int j = 0; j < N; j++)
                B[i][j] = scan.nextInt();

        int[][] C = s.multiply(A, B);                            		// 슈트라센 알고리즘에 두 행렬을 대입

        System.out.println("두 행렬의 곱 : ");                    		// 두 행렬의 곱 결과 출력
        for (int i = 0; i < N; i++)
        {
            for (int j = 0; j < N; j++)
                System.out.print(C[i][j] +" ");
            System.out.println();
        }

    }
}

```


### 4.1 main 메소드

```java
public static void main (String[] args)
    {
        Scanner scan = new Scanner(System.in);


        Strassen s = new Strassen();

        System.out.println("행렬크기를 입력하세요 (n) :");
        int N = scan.nextInt();

        System.out.println("첫 번째 행렬의 값들을 입력하세요");        // 첫 번째 행렬 A에 대한 값들을 입력받음
        int[][] A = new int[N][N];
        for (int i = 0; i < N; i++)
            for (int j = 0; j < N; j++)
                A[i][j] = scan.nextInt();

        System.out.println("두 번째 행렬의 값들을 입력하세요1");       // 두 번째 행렬 B에 대한 값들을 입력받음
        int[][] B = new int[N][N];
        for (int i = 0; i < N; i++)
            for (int j = 0; j < N; j++)
                B[i][j] = scan.nextInt();

        int[][] C = s.multiply(A, B);                            		// 슈트라센 알고리즘에 두 행렬을 대입

        System.out.println("두 행렬의 곱 : ");                     	// 두 행렬의 곱 결과 출력
        for (int i = 0; i < N; i++)
        {
            for (int j = 0; j < N; j++)
                System.out.print(C[i][j] +" ");
            System.out.println();
        }

    }
```

슈트라센 알고리즘을 통해 두개의 행렬을 실제로 실행하는 메소드로, n의 크기를 입력해 행렬의 크기를 직접 지정하고, 두 개의 행렬 값을 입력받는다
입력받은 값들은 행렬의 곱을 해줄 multiply 메소드를 통해 연산을 하고 그 결과를 출력해준다. 



### 4.2 multiply메소드

```java
  
public int[][] multiply(int[][] A, int[][] B)        			// 두 개의 nxn 정사각행렬의 곱을 계산하는 메소드 생성
    {
        int n = A.length;                               			// 2차원 배열의 행렬의 길이를 n에 저장( nxn 정사각행렬을 만드는데 필요)
        int[][] R = new int[n][n];                       			// 슈트라센 알고리즘을 적용하고 난 후의 결과값을 저장할 Result행렬 생성

        if (n < 2)                                        			// 임계차원 보다 작으면 기존 메트릭스 곱으로 풀이
            R[0][0] = A[0][0] * B[0][0];
        else
        {
            int[][] A11 = new int[n/2][n/2];             		// 첫번째 행렬에서 소행렬들의 크기를 정의해줌
            int[][] A12 = new int[n/2][n/2];
            int[][] A21 = new int[n/2][n/2];
            int[][] A22 = new int[n/2][n/2];

            int[][] B11 = new int[n/2][n/2];             		// 두번째 행렬에서 소행렬들의 크기를 정의해줌
            int[][] B12 = new int[n/2][n/2];
            int[][] B21 = new int[n/2][n/2];
            int[][] B22 = new int[n/2][n/2];

                                                              			 // 첫번째 행렬을 4개의 소행렬로 나눈다
            divideMatrics(A, A11, 0 , 0);
            divideMatrics(A, A12, 0 , n/2);
            divideMatrics(A, A21, n/2, 0);
            divideMatrics(A, A22, n/2, n/2);

                                                             			 // 두번째 행렬을 4개의 소행렬로 나눈다
            divideMatrics(B, B11, 0 , 0);
            divideMatrics(B, B12, 0 , n/2);
            divideMatrics(B, B21, n/2, 0);
            divideMatrics(B, B22, n/2, n/2);


            //슈트라센 알고리즘에서 사용하는 A와 B로 이루어진 새로은 행렬 M을 다음과 같이 적용시킨다
         
            int [][] M1 = multiply(add(A11, A22), add(B11, B22));         //  M1 = (A11 + A22)(B11 + B22)
            int [][] M2 = multiply(add(A21, A22), B11);                      //  M2 = (A21 + A22) B11
            int [][] M3 = multiply(A11, sub(B12, B22));                      //  M3 = A11 (B12 - B22)
            int [][] M4 = multiply(A22, sub(B21, B11));                      //  M4 = A22 (B21 - B11)
            int [][] M5 = multiply(add(A11, A12), B22);                      //  M5 = (A11 + A12) B22
            int [][] M6 = multiply(sub(A21, A11), add(B11, B12));         //  M6 = (A21 - A11) (B11 + B12)
            int [][] M7 = multiply(sub(A12, A22), add(B21, B22));         //  M7 = (A12 - A22) (B21 + B22)


            // AxB=C인 행렬 C를 행렬 M으로 다음과 같이 표현한다
           
            int [][] C11 = add(sub(add(M1, M4), M5), M7);    		// C11 = M1 + M4 - M5 + M7
            int [][] C12 = add(M3, M5);                            		// C12 = M3 + M5
            int [][] C21 = add(M2, M4);                            		// C21 = M2 + M4
            int [][] C22 = add(sub(add(M1, M3), M2), M6);   		//  C22 = M1 - M2 + M3 + M6

            // 각 AxB의 연산이 끝난 소행렬들을 결과를 나타내는 행렬 R행렬로 합쳐준다.
            put(C11, R, 0 , 0);
            put(C12, R, 0 , n/2);
            put(C21, R, n/2, 0);
            put(C22, R, n/2, n/2);
        }
        return R;                                              		//결과값을 반환
    }
   
```

슈트라센 알고리즘을 통해 행렬의 곱을 해주는 메소드.
입력받은 행렬 A와 B를 각각의 소행렬 4개로 나누고 그 값들을 통해 아래 연산을 하고 그 결과를 행렬 R에 다시 합친다.

             M1 = (A11 + A22)(B11 + B22)
             M2 = (A21 + A22) B11
             M3 = A11 (B12 - B22)
             M4 = A22 (B21 - B11)
             M5 = (A11 + A12) B22
             M6 = (A21 - A11) (B11 + B12)
             M7 = (A12 - A22) (B21 + B22)
             


             C11 = M1 + M4 - M5 + M7
             C12 = M3 + M5
             C21 = M2 + M4
             C22 = M1 - M2 + M3 + M6
             


### 4.3 divideMatrics , put메소드

```java

public void divideMatrics(int[][] P, int[][] C, int iB, int jB)
    {
        for(int i1 = 0, i2 = iB; i1 < C.length; i1++, i2++)
            for(int j1 = 0, j2 = jB; j1 < C.length; j1++, j2++)
                C[i1][j1] = P[i2][j2];
    }

    // 각 소행렬들의 연산이 끝난 후 그 값들을 하나의 행렬로 다시 합춰준다( divideMatrics메소드와 반대 역할)
    public void put(int[][] C, int[][] P, int iB, int jB)
    {
        for(int i1 = 0, i2 = iB; i1 < C.length; i1++, i2++)
            for(int j1 = 0, j2 = jB; j1 < C.length; j1++, j2++)
                P[i2][j2] = C[i1][j1];
    }
 
```

divideMatrics 메소드는 각 행렬을 4개의 소행렬로 나누어준다. put메소드는 divideMatrics메소드와 반대 역할을 하며
행렬곱이 끝난 소행렬들을 다시 하나의 행렬로 합쳐주는 메소드

### 4.4 add, sub메소드

```java
 // 두 행렬을 더해주는 메소드
    public int[][] add(int[][] A, int[][] B)
    {
        int n = A.length;
        int[][] C = new int[n][n];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                C[i][j] = A[i][j] + B[i][j];
        return C;
    }

    // 두행렬을 빼주는 메소드
    public int[][] sub(int[][] A, int[][] B)
    {
        int n = A.length;
        int[][] C = new int[n][n];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                C[i][j] = A[i][j] - B[i][j];
        return C;
    }


```
add메소드는 두 행렬을 더해주는 메소드, sub메소드는 두 행렬을 빼주는 메소드

### 5. 실행결과
n=1
![스크린샷(37)](https://user-images.githubusercontent.com/81409594/116369189-0f211180-a844-11eb-99c3-db60d2bfae5f.png)

n=4
![스크린샷(38)](https://user-images.githubusercontent.com/81409594/116369246-1ea05a80-a844-11eb-94f9-79eec83fbda8.png)

n=8
![스크린샷(39)](https://user-images.githubusercontent.com/81409594/116369307-31b32a80-a844-11eb-9abf-776b71146815.png)

n=16
![스크린샷(40)](https://user-images.githubusercontent.com/81409594/116369335-3972cf00-a844-11eb-866a-f418f92c3e80.png)




## 6.참조 
[https://slidesplayer.org/slide/16397992/](https://slidesplayer.org/slide/16397992/)

[https://skylove1982.tistory.com/302](https://skylove1982.tistory.com/302)

[https://ko.wikipedia.org/wiki/%EC%8A%88%ED%8A%B8%EB%9D%BC%EC%84%BC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98](https://ko.wikipedia.org/wiki/%EC%8A%88%ED%8A%B8%EB%9D%BC%EC%84%BC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)








        
        

