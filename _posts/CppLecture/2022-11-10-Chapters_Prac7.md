---
title: "[C++ Programming] 실습Part2 : 문자열 다루기 기본"
excerpt: "[C++ Programming] 11주차 1차시"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 문자열 다루기 기본

+ 📝문제 설명 

    + 문자열 s의 길이가 4 혹은 6이고, 숫자로만 구성돼있는지 확인해주는 함수, solution을 완성하세요.  
    예를 들어 s가 "a234"이면 False를 리턴하고 "1234"라면 True를 리턴하면 됩니다.

<br/>

+ ⚠️제한사항
    + s는 길이 1 이상, 길이 8 이하인 문자열입니다.
    + s는 영문 알파벳 대소문자 또는 0부터 9까지 숫자로 이루어져 있습니다.
    
<br/>

+ 📜입출력 예

   |            s            |       result      | 
   | :----------------------: | :---------------: | 
   |           "a234"     |     	false |  
   |      "1234"            |    	true      |  

<br/>

+ ✏️코드

+ 내코드

```cpp
    #include <string>
    #include <vector>

    using namespace std;

    bool isCorrect(string s) {
        int len = s.length();

        if (len != 4 && len != 6) {
            return false;
        }

        for (int i = 0; i < len; i++) {
            if (s[i] < '0' || s[i] > '9') {
                return false;
            }
        }

        return true;
    }

    bool solution(string s) {
        bool answer = true;

        answer = isCorrect(s);

        return answer;
    }

    int main(void) {
        string str = "1234";
        solution(str);

        return 0;
    }
```

+ 교수님 코드

    + **문자열 클래스 구현해보기**

```cpp

```