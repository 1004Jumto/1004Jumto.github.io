---
title: "[C++ Programming] 실습 17, 18"
excerpt: "[C++ Programming] 7주차 1차시"
categories: [Cpp]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 💻실습

### 17. 짝수의 합

+ 📝문제 설명 

    + 정수 n이 주어질 때, n이하의 짝수를 모두 더한 값을 return 하도록 solution 함수를 작성해주세요.

<br/>

+ ⚠️제한사항
    + 0 < n ≤ 1000

<br/>

+ 📜입출력 예

```
    n  | result |
    10 |   30   |
    4  |   6    |
```
   + 입출력 예 #1 : n이 10이므로 2 + 4 + 6 + 8 + 10 = 30을 return 합니다.
   + 입출력 예 #2 : n이 4이므로 2 + 4 = 6을 return 합니다. 

<br/>

+ ✏️코드

```cpp
    #include <string>
    #include <vector>

    using namespace std;

    int solution(int n) {
        int answer = 0;
        
        for(int i=1; i<=n; i++){
            if(i%2 == 0){
                answer += i;
            }
        }
        return answer;
    }
```



### 18. 외계행성의 나이

+ 📝문제 설명 

    + 우주여행을 하던 머쓱이는 엔진 고장으로 PROGRAMMERS-962 행성에 불시착하게 됐습니다. 입국심사에서 나이를 말해야 하는데, PROGRAMMERS-962 행성에서는 나이를 알파벳으로 말하고 있습니다. a는 0, b는 1, c는 2, ..., j는 9입니다. 예를 들어 23살은 cd, 51살은 fb로 표현합니다. 나이 age가 매개변수로 주어질 때 PROGRAMMER-962식 나이를 return하도록 solution 함수를 완성해주세요.

<br/>

+ ⚠️제한사항
    + age는 자연수입니다.
    + age ≤ 1,000
    + PROGRAMMERS-962 행성은 알파벳 소문자만 사용합니다.

<br/>

+ 📜입출력 예

```
    age   |  result  |
     23   |   "cd"   |
     51   |   "fb"   |
     100  |   "baa"  |
```
   + 입출력 예 #1 : age가 23이므로 "cd"를 return합니다.
   + 입출력 예 #2 : age가 51이므로 "fb"를 return합니다.
   + 입출력 예 #3 : age가 100이므로 "baa"를 return합니다.

<br/>

+ ✏️코드

```cpp
    #include <string>
    #include <vector>

    using namespace std;

    /////////////// My Solution //////////////
    string solution(int age) {
        string answer = "";
        vector<string> str;
        string sage[10] = { "a", "b", "c", "d", "e", "f", "g", "h", "i", "j" };

        while(age != 0){
            str.push_back(sage[age%10]);    
            age /= 10;
        }
        
        for(int i = str.size() - 1; i >= 0; i--){
            answer += str[i];
        }

        return answer;
    }


    /////////// Professor Solution ///////////
    string solution(int age){
        string answer = "";

        for(int i = age; i > 0; i /= 10){
            answer = (char)('a' + (i % 10)) + answer;       // 형변환(type casting) : 어떻게 해석할 지에 따라 의미가 달라짐 
                                                            // 명시적 형 변환 : 해석 의미를 알려줌
        }

        return answer;
    }
```
