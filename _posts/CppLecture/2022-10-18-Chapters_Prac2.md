---
title: "[C++ Programming] 실습Part1 : 19.각도기, 20.최빈값구하기"
excerpt: "[C++ Programming] 7주차 2차시"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 💻실습
  
### 19. 각도기

+ 📝문제 설명 

    + 각에서 0도 초과 90도 미만은 예각, 90도는 직각, 90도 초과 180도 미만은 둔각 180도는 평각으로 분류합니다. 
    + 각 angle이 매개변수로 주어질 때 예각일 때 1, 직각일 때 2, 둔각일 때 3, 평각일 때 4를 return하도록 solution 함수를 완성해주세요.

    ```
        예각 : 0 < angle < 90
        직각 : angle = 90
        둔각 : 90 < angle < 180
        평각 : angle = 180
    ```
<br/>

+ ⚠️제한사항
    + 0 < angle ≤ 180
    + angle은 정수입니다.
    
<br/>

+ 📜입출력 예

   | angle  | result |
   | :----: | :----: |
   |   70   |   1    |
   |   91   |   3    |
   |  180   |   4    |

   + 입출력 예 #1 : angle이 70이므로 예각입니다. 따라서 1을 return합니다.
   + 입출력 예 #2 : angle이 91이므로 둔각입니다. 따라서 3을 return합니다.
   + 입출력 예 #3 : angle이 180이므로 평각입니다. 따라서 4를 return합니다.

<br/>

+ ✏️코드

```cpp
    #include <string>
    #include <vector>

    using namespace std;

    int solution(int angle) {

        if(angle == 180)
            return 4;
        
        else if(angle > 90)
            return 3;
       
        else if(angle == 90)
            return 2;
       
        else return 1;

    }
```

### 19. 최빈값 구하기

+ 📝문제 설명 

    + 최빈값은 주어진 값 중에서 가장 자주 나오는 값을 의미합니다.  
    + 정수 배열 array가 매개변수로 주어질 때, 최빈값을 return 하도록 solution 함수를 완성해보세요.
    + 최빈값이 여러 개면 -1을 return 합니다.

<br/>

+ ⚠️제한사항
    + 0 < array의 길이 < 100
    + 0 ≤ array의 원소 < 1000

<br/>

+ 📜입출력 예

|       array        | result |
| :----------------: | :----: |
| [1, 2, 3, 3, 3, 4] |   3    |
|    [1, 1, 2, 2]    |   -1   |
|        [1]         |   1    |

   + 입출력 예 #1 : [1, 2, 3, 3, 3, 4]에서 1은 1개 2는 1개 3은 3개 4는 1개로 최빈값은 3입니다.
   + 입출력 예 #2 : [1, 1, 2, 2]에서 1은 2개 2는 2개로 최빈값이 1, 2입니다. 최빈값이 여러 개이므로 -1을 return 합니다.
   + 입출력 예 #3 : [1]에는 1만 있으므로 최빈값은 1입니다.

<br/>

+ ✏️코드

```cpp
    #include <string>
    #include <vector>

    using namespace std;

    int solution(vector<int> array) {
        vector<int> count;
        
        for(int i = 0; i < 1000; i++){
            count.push_back(0);
        }
        
        for(int i = 0; i < array.size(); i++){
            count[array[i]]++;
        }
        
        // 제일 count가 큰 애의 array값이 최빈값
        int max = 0;
        int mode = 0;
        int cnt = 1;
        for(int i = 0; i < 1000; i++){
            if(max <= count[i]){
                if(max == count[i])     // 빈도가 같은 경우
                    cnt++;
                else{
                    cnt = 1;
                }
                max = count[i];
                mode = i;
            }
        }
        
        if(cnt > 1)
            return -1;
        
        return mode;
    }
```

+ Professor Solution

```cpp
    #include <string>
    #include <vector>
    #include <algorithm>

    using namespace std;

    int solution(vector<int> array)
    {
        int answer = 0;
        int count[1000];

        int mode;
        int modeCount = -1;
        bool duplicate = false;

        // 0으로 초기화
        for (int i = 0; i < 1000; i++)
            count[i] = 0;
        // array에 있는 숫자를 인덱스로 간주하여 그 숫자가 나올때마다 ++을 하는 형식
        for (int i = 0; i < array.size(); i++)
            count[array[i]]++;

        for (int i = 0; i < 1000; i++)
        {
            // max 구하기
            if (count[i] > modeCount)
            {
                mode = i;
                modeCount = count[i];
                duplicate = false; // 동점자인지 아닌지
            }
            else if (count[i] == modeCount)
            {
                duplicate = true;
            }
        }
        if (!duplicate)
            answer = mode;
        else
            answer = -1;

        return answer;
    }
```