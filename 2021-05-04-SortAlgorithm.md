# Sort Algorithm




## 1. 버블정렬
![https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Bubblesort-edited-color.svg/1024px-Bubblesort-edited-color.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Bubblesort-edited-color.svg/1024px-Bubblesort-edited-color.svg.png)
거품 정렬은 두 인접한 원소를 검사하여 정렬하는 방법이다. 시간 복잡도가 O(n^2)로 상당히 느리지만, 코드가 단순하기 때문에 자주 사용된다. 
원소의 이동이 거품이 수면으로 올라오는 듯한 모습을 보이기 때문에 지어진 이름이다.


### 1. (1) 알고리즘 구현

```java
package sort;

public class BubbleSort {
    public static void main(String[] args) {
        int[] a = {254,3,213,64,75,56,4,324,65,78,9,5,76,3410,8,342,76,1,2,7,10,123,58,69,73,33,2748,1264,894,37,11,
                333,489,126,4456,231,4568,222,4687,88,100,230,564,79,123,535,2555,1111,120,357,2333,108,101,206,3
                ,200,300,400,500,600,700,800,900,1000,2000,3000,4000,5000,6000,7000,8000,9000,123,124,125,126,127,
                128,129,130,1341,132,133,134,135,136,137,138,139,140,141,142,143,144,145,146,201,202,203,204,205,
                 307,356,355};
        int b;
        for(int i = 0 ; i < a.length ; i ++) {
            for(int j = 0 ; j < a.length -i -1 ; j ++) {
                if(a[j]>a[j+1]) {
                    b = a[j];
                    a[j] = a[j+1];
                    a[j+1] = b;
                }
            }
        }

        for(int i = 0 ; i < a.length ; i ++) {
            System.out.println(a[i]);
        }
    }
}

```

### 1. (2) 결과

![버블정렬결과 (2)](https://user-images.githubusercontent.com/81409594/116986503-d16f2d80-ad08-11eb-8700-e354b2f1284a.png)



## 2. 선택정렬
![https://upload.wikimedia.org/wikipedia/commons/b/b0/Selection_sort_animation.gif](https://upload.wikimedia.org/wikipedia/commons/b/b0/Selection_sort_animation.gif)
선택 정렬은 제자리 정렬 알고리즘의 하나로, 다음과 같은 순서로 이루어진다.

1. 주어진 리스트 중에 최소값을 찾는다.
2. 그 값을 맨 앞에 위치한 값과 교체한다(패스(pass)).
3. 맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.

비교하는 것이 상수 시간에 이루어진다는 가정 아래, n개의 주어진 리스트를 이와 같은 방법으로 정렬하는 데에는 Θ(n^2) 만큼의 시간이 걸린다.

선택 정렬은 알고리즘이 단순하며 사용할 수 있는 메모리가 제한적인 경우에 사용시 성능 상의 이점이 있다.


### 2. (1) 알고리즘 구현

```java
package sort;

public class Selection {
    public void sort(int[] data){
        int size = data.length;
        int min; //최소값을 가진 데이터의 인덱스 저장 변수
        int temp;

        for(int i=0; i<size-1; i++){ // size-1 : 마지막 요소는 자연스럽게 정렬됨
            min = i;
            for(int j=i+1; j<size; j++){
                if(data[min] > data[j]){
                    min = j;
                }
            }
            temp = data[min];
            data[min] = data[i];
            data[i] = temp;
        }
    }
    public static void main(String[] args) {

        Selection selection = new Selection();

        int data[] = {254,3,213,64,75,56,4,324,65,78,9,5,76,3410,8,342,76,1,2,7,10,123,58,69,73,33,2748,1264,894,37,11,
                333,489,126,4456,231,4568,222,4687,88,100,230,564,79,123,535,2555,1111,120,357,2333,108,101,206,3
                ,200,300,400,500,600,700,800,900,1000,2000,3000,4000,5000,6000,7000,8000,9000,123,124,125,126,127,
                128,129,130,1341,132,133,134,135,136,137,138,139,140,141,142,143,144,145,146,201,202,203,204,205,
                307,356,355};

        selection.sort(data);

        for(int i=0; i<data.length; i++){
            System.out.println(data[i]);
        }
    }
}
```

### 2. (2) 결과

![선택정렬 (2)](https://user-images.githubusercontent.com/81409594/116986563-e51a9400-ad08-11eb-995f-d41c43184cca.png)




## 3. 삽입정렬
삽입 정렬은 자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘이다.

k번째 반복 후의 결과 배열은, 앞쪽 k + 1 항목이 정렬된 상태이다.

![https://upload.wikimedia.org/wikipedia/commons/3/32/Insertionsort-before.png](https://upload.wikimedia.org/wikipedia/commons/3/32/Insertionsort-before.png)

각 반복에서 정렬되지 않은 나머지 부분 중 첫 번째 항목은 제거되어 정확한 위치에 삽입된다. 그러므로 다음과 같은 결과가 된다.

![https://upload.wikimedia.org/wikipedia/commons/d/d9/Insertionsort-after.png](https://upload.wikimedia.org/wikipedia/commons/d/d9/Insertionsort-after.png)

배열이 길어질수록 효율이 떨어진다. 다만, 구현이 간단하다는 장점이 있다.

선택 정렬이나 거품 정렬과 같은 같은 O(n2) 알고리즘에 비교하여 빠르며, 안정 정렬이고 in-place 알고리즘이다.


### 3. (1) 알고리즘 구현

```java
package sort;

public class InsertionSort {

    public static void main(String[] args) {

        int [] arr = {254,3,213,64,75,56,4,324,65,78,9,5,76,3410,8,342,76,1,2,7,10,123,58,69,73,33,2748,1264,894,37,11,
                333,489,126,4456,231,4568,222,4687,88,100,230,564,79,123,535,2555,1111,120,357,2333,108,101,206,3
                ,200,300,400,500,600,700,800,900,1000,2000,3000,4000,5000,6000,7000,8000,9000,123,124,125,126,127,
                128,129,130,1341,132,133,134,135,136,137,138,139,140,141,142,143,144,145,146,201,202,203,204,205,
                307,356,355};



        for (int i = 1; i < arr.length; i++) {

            int standard = arr[i];  // 기준

            int aux = i - 1;   // 비교할 대상



            while (aux >= 0 && standard < arr[aux]) {

                arr[aux + 1] = arr[aux];   // 비교대상이 큰 경우 오른쪽으로 밀어냄

                aux--;

            }

            arr[aux + 1] = standard;  // 기준값 저장

        }

        printArr(arr);

    }



    public static void printArr(int[] arr) {

        for (int i = 0; i < arr.length; i++) {

            System.out.println(arr[i] + " ");

        }

    }

}
```

### 3. (2) 결과

![삽입정렬 (2)](https://user-images.githubusercontent.com/81409594/116986590-ed72cf00-ad08-11eb-9570-64e3168b0083.png)




## 4. 쉘정렬

![https://upload.wikimedia.org/wikipedia/commons/d/d8/Sorting_shellsort_anim.gif](https://upload.wikimedia.org/wikipedia/commons/d/d8/Sorting_shellsort_anim.gif)

셸 정렬은 가장 오래된 정렬 알고리즘의 하나이다. 이름은 1959년 이 방법을 발표한 창안자 도널드 셸의 이름을 따서 붙여졌다. 셸 정렬은 개념을 이해하고 구현하기는 쉬우나 시간복잡도 분석은 조금 복잡하다.

셸 정렬은 다음과 같은 삽입 정렬의 성질을 이용, 보완한 삽입정렬의 일반화로 볼 수 있다.

-삽입 정렬은 입력되는 초기리스트가 "거의 정렬"되어 있을 경우 효율적이다.
-삽입 정렬은 한 번에 한 요소의 위치만 결정되기 때문에 비효율적이다.

셸 정렬은 주어진 자료 리스트를 특정 매개변수 값의 길이를 갖는 부파일(subfile)로 쪼개서, 각 부파일에서 정렬을 수행한다. 즉, 매개변수 값에 따라 부파일(Subfile)이 발생하며, 매개변수값을 줄이며 이 과정을 반복하고 결국 매개변수 값이 1이면 정렬은 완성된다.

셸 정렬은 다음과 같은 과정으로 나눈다.

1. 데이터를 십수 개 정도 듬성듬성 나누어서 삽입 정렬한다.
2. 데이터를 다시 잘게 나누어서 삽입 정렬한다.
3. 이렇게 계속 하여 마침내 정렬이 된다.


### 4. (1) 알고리즘 구현

```java
package sort;

import java.util.Arrays;

public class ShellSort {

    public static void main(String[] args) {

        int[] arr = {254, 3, 213, 64, 75, 56, 4, 324, 65, 78, 9, 5, 76, 3410, 8, 342, 76, 1, 2, 7, 10, 123, 58, 69, 73, 33, 2748, 1264, 894, 37, 11,
                333, 489, 126, 4456, 231, 4568, 222, 4687, 88, 100, 230, 564, 79, 123, 535, 2555, 1111, 120, 357, 2333, 108, 101, 206, 3
                , 200, 300, 400, 500, 600, 700, 800, 900, 1000, 2000, 3000, 4000, 5000, 6000, 7000, 8000, 9000, 123, 124, 125, 126, 127,
                128, 129, 130, 1341, 132, 133, 134, 135, 136, 137, 138, 139, 140, 141, 142, 143, 144, 145, 146, 201, 202, 203, 204, 205,
                307, 356, 355};
        int len = arr.length;

        int temp = 0;
        int gap = len;
        for (; gap > 1; ) {
            gap = (gap / 3) + 1;

            for (int i = 0; i < gap; i++) {// gap 만큼 반복한다
                //삽입 정렬 로직
                for (int j = i + gap; j < len; j += gap) {
            

                    for (int x = i; x < j; x += gap) {
                 
                        if (arr[x] > arr[j]) {
                            temp = arr[x];
                            arr[x] = arr[j];
                            arr[j] = temp;
                        }
                    }
                }
            }


        }
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
    }
}
```

### 4. (2) 결과

![쉘정렬 (2)](https://user-images.githubusercontent.com/81409594/116986616-f5cb0a00-ad08-11eb-9691-ee5fc036f6a5.png)














## 6.참조 
[https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC](https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC)

[https://ko.wikipedia.org/wiki/%EC%84%A0%ED%83%9D_%EC%A0%95%EB%A0%AC](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%83%9D_%EC%A0%95%EB%A0%AC)

[https://ko.wikipedia.org/wiki/%EC%82%BD%EC%9E%85_%EC%A0%95%EB%A0%AC](https://ko.wikipedia.org/wiki/%EC%82%BD%EC%9E%85_%EC%A0%95%EB%A0%AC)

[https://ko.wikipedia.org/wiki/%EC%85%B8_%EC%A0%95%EB%A0%AC](https://ko.wikipedia.org/wiki/%EC%85%B8_%EC%A0%95%EB%A0%AC)

[








        
        


