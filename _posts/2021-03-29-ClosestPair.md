# H_Group

---
layout: single
title:  "Welcome to H_Group!"
date:   2021-03-27 10:56:38 +0900
categories: jekyll update
---

{% highlight ruby %}
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
 
public class test {
    public static void main(String[] args) {
        ArrayList<Integer> numberList = new ArrayList<Integer>();
 
        try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in))) {
            String input = br.readLine();
            String[] numStrList = input.split(" ");
            for (String numStr : numStrList) {
                numberList.add(Integer.parseInt(numStr));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
 
        Collections.sort(numberList);       //정렬
        Collections.reverse(numberList);    //내림차순 정렬
 
        //완전 탐색 알고리즘
        int minDist = Integer.MAX_VALUE;
        int temp;
        for(int i=0; i<numberList.size()-1; i++){
            for(int j=i+1; j<numberList.size(); j++){
                temp = distance(numberList.get(i), numberList.get(j));
                if (temp < minDist) {
                    minDist = temp;
                    System.out.println("minDist = "+minDist);
                    System.out.println("result = "+numberList.get(i)+", "+numberList.get(j));
                }
                
            }
        }
    }
    
    public static int distance(int p, int q) {
        int dis = p-q;
        return dis;
    }

{% endhighlight %}
//출처: https://kimna-y.tistory.com/1 [나의 흔적들]
