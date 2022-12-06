---
title: "[C++ Programming] 실습Part2 : 1.같은 숫자는 싫어,  2.최소직사각형"
excerpt: "[C++ Programming] 9주차 1차시"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 1. 같은 숫자는 싫어

+ 📝문제 설명 

    + 배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있습니다. 이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 합니다. 단, 제거된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다.  
    + 예를 들면, arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return 합니다.  
    arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return 합니다.  
    + 배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 return 하는 solution 함수를 완성해 주세요.

<br/>

+ ⚠️제한사항
    + 배열 arr의 크기 : 1,000,000 이하의 자연수
    + 배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수

    
<br/>

+ 📜입출력 예

   |            arr             |       answer      | 
   | :------------------------: | :---------------: | 
   |     [1,1,3,3,0,1,1]        |      [1,3,0,1]    |  
   |      [4,4,4,3,3]           |       [4,3]       |  

<br/>

+ ✏️코드

+ 내 코드

```cpp
    #include <vector>
    #include <iostream>

    using namespace std;

    vector<int> solution(vector<int> arr) 
    {
        vector<int> answer;

        for(int i=0; i<arr.size()-1; i++){
            if(arr[i] != arr[i+1]){
                answer.push_back(arr[i]);
            }
        }
        answer.push_back(arr[arr.size()-1]);

        return answer;
    }
```

+ 교수님 코드

```cpp
    #include <vector>
    #include <iostream>

    using namespace std;

    vector<int> solution(vector<int> arr) 
    {
        vector<int> answer;

        answer.push_back(arr[0]);
        for(int i=1; i<arr.size(); i++){
            if(arr[i] != arr[i-1]){
                answer.push_back(arr[i]);
            }
        }
        
        return answer;
    }
```

<br/>

## 2. 최소직사각형

+ 📝문제 설명 

    + 명함 지갑을 만드는 회사에서 지갑의 크기를 정하려고 합니다. 다양한 모양과 크기의 명함들을 모두 수납할 수 있으면서, 작아서 들고 다니기 편한 지갑을 만들어야 합니다. 이러한 요건을 만족하는 지갑을 만들기 위해 디자인팀은 모든 명함의 가로 길이와 세로 길이를 조사했습니다.
    
    + 아래 표는 4가지 명함의 가로 길이와 세로 길이를 나타냅니다.

   | 명함번호 |  가로   |  세로  | 
   | :-----: | :----:  | :----: | 
   |    1    |    60   |   50   | 
   |    2    |    30   |   70   |
   |    3    |    60   |   30   |
   |    4    |    80   |   40   | 

   + 가장 긴 가로 길이와 세로 길이가 각각 80, 70이기 때문에 80(가로) x 70(세로) 크기의 지갑을 만들면 모든 명함들을 수납할 수 있습니다. 하지만 2번 명함을 가로로 눕혀 수납한다면 80(가로) x 50(세로) 크기의 지갑으로 모든 명함들을 수납할 수 있습니다. 이때의 지갑 크기는 4000(=80 x 50)입니다.  
   
   + 모든 명함의 가로 길이와 세로 길이를 나타내는 2차원 배열 sizes가 매개변수로 주어집니다. 모든 명함을 수납할 수 있는 가장 작은 지갑을 만들 때, 지갑의 크기를 return 하도록 solution 함수를 완성해주세요.


<br/>

+ ⚠️제한사항

    + sizes의 길이는 1 이상 10,000 이하입니다.
    + sizes의 원소는 [w, h] 형식입니다.
    + w는 명함의 가로 길이를 나타냅니다.
    + h는 명함의 세로 길이를 나타냅니다.
    + w와 h는 1 이상 1,000 이하인 자연수입니다.
    
<br/>

+ 📜입출력 예

   | denum1  |  num1  |
   | :----:  | :----: | 
   |    1    |    2   | 
   |    9    |    2   | 

   + 입출력 예 #1 : 문제 예시와 같습니다
   + 입출력 예 #2 : 명함들을 적절히 회전시켜 겹쳤을 때, 3번째 명함(가로: 8, 세로: 15)이 다른 모든 명함보다 크기가 큽니다. 따라서 지갑의 크기는 3번째 명함의 크기와 같으며, 120(=8 x 15)을 return 합니다.
   + 입출력 예 #3 : 명함들을 적절히 회전시켜 겹쳤을 때, 모든 명함을 포함하는 가장 작은 지갑의 크기는 133(=19 x 7)입니다.

<br/>

+ ✏️코드

+ 내 코드

```cpp
    #include <string>
    #include <vector>

    using namespace std;

    int solution(vector<vector<int>> sizes) {
        int answer = 0;
        
        // 큰 값을 왼쪽으로 놓기
        for(int i=0; i < sizes.size(); i++){
                if(sizes[i][0] < sizes[i][1]){
                    int tmp = sizes[i][0];
                    sizes[i][0] = sizes[i][1];
                    sizes[i][1] = tmp;
                }
        }
        
        // 젤 큰 값 찾기 
        int max0 = 0, max1 = 0;
        for(int i=0; i<sizes.size(); i++){
            if(sizes[i][0] >= max0){
                max0 = sizes[i][0];
            }
            if(sizes[i][1] >= max1){
                max1 = sizes[i][1];
            }
        }
        
        answer = max0 * max1;
        return answer;
    }
```

+ 교수님 코드

```cpp
    #include <string>
    #include <vector>

    using namespace std;

    int solution(vector<vector<int>> sizes) {
        int answer = 0;
        
        int minmax = -1;
        int maxmax = -1;
        for(int i=0; i<sizes.size(); i++){
            if(sizes[i][0] > sizes[i][1]){
                if(sizes[i][0] > maxmax) maxmax = sizes[i][0];
                if(sizes[i][1] > minmax) minmax = sizes[i][1];
            }else{
                if(sizes[i][1] > maxmax) maxmax = sizes[i][1];
                if(sizes[i][0] > minmax) minmax = sizes[i][0];
            }
        }
        answer = minmax * maxmax;
        return answer;
    }
```