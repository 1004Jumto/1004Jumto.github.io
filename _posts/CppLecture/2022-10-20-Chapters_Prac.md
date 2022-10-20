---
title: "[C++ Programming] 실습 22, 23"
excerpt: "[C++ Programming] 8주차 1차시"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 💻실습

### 22. 분수의 덧셈

+ 📝문제 설명 

    + 첫 번째 분수의 분자와 분모를 뜻하는 denum1, num1, 두 번째 분수의 분자와 분모를 뜻하는 denum2, num2가 매개변수로 주어집니다. 두 분수를 더한 값을 기약 분수로 나타냈을 때 분자와 분모를 순서대로 담은 배열을 return 하도록 solution 함수를 완성해보세요.

입출력 예
denum1	num1	denum2	num2	result
1	2	3	4	[5, 4]
9	2	1	3	[29, 6]

<br/>

+ ⚠️제한사항
    + 0 <denum1, num1, denum2, num2 < 1,000
    
<br/>

+ 📜입출력 예

   | denum1  |  num1  | denum2 |  num2  | result |
   | :----:  | :----: | :----: | :----: | :----: |
   |    1    |    2   |   3    |    4   | [5, 4] |  
   |    9    |    2   |   1    |    3   | [29, 6]| 


   + 입출력 예 #1 : 1 / 2 + 3 / 4 = 5 / 4입니다. 따라서 [5, 4]를 return 합니다.
   + 입출력 예 #2 : 9 / 2 + 1 / 3 = 29 / 6입니다. 따라서 [29, 6]을 return 합니다.

<br/>

+ ✏️코드

+ 내 코드

```cpp
    #include <string>
    #include <vector>

    using namespace std;

    int getCommonDivisor(int denum,int num);
    vector<int> solution(int denum1, int num1, int denum2, int num2) {
        vector<int> answer;
        
        // 통분한 분자 분모
        int denum = denum1 * num2 + denum2 * num1;
        int num = num1 * num2;
        
        // 공약수 구하기
        int gcd = getCommonDivisor(denum, num);
        while(gcd != 1){
            denum /= gcd;
            num /= gcd;
            gcd = getCommonDivisor(denum, num);
        }
        
        answer.push_back(denum);
        answer.push_back(num);
        
        return answer;
    }

    int getCommonDivisor(int denum, int num){
        int r;
        
        while(num != 0){
            r = denum%num;
            if(r == 0) return num;
            
            denum = num;
            num = r;   
        }
        
        return denum;
    }
```

+ 교수님 코드

```cpp
    #include <string>
    #include <vector>

    using namespace std;

    // 두 개 이상의 리턴 value가 필요할 때 call by ref는 유용하다
    void getSum(int &denum, int &num, int denum1, int num1, int denum2, int num2);
    void postDiv(int &denum, int &num);

    vector<int> solution(int denum1, int num1, int denum2, int num2) {
        vector<int> answer;
        int denum, num;

        getSum(denum, num, denum1, num1, denum2, num2);
        postDiv(denum, num);

        answer.push_back(denum);
        answer.push_back(num);

        return answer;
    }

    void getSum(int &denum, int &num, int denum1, int num1, int denum2, int num2){
        num  = num1 * num2;
        denum = denum1 * num2 + denum2 * num1;
    }

    void postDiv(int &denum, int &num){
        for(int i = 2; denum > 1 && num > 1 && denum >= i && num >= i; ){
            if(denum % i == 0 && num % i == 0){
                denum = denum / i;
                num = num / i;
            }
            else 
                i++;
        }
    }

```

<br/>

### 23. 팰린드롬

+ 📝문제 설명 

    + 앞에서부터 읽을 때와 뒤에서부터 읽을 때 똑같은 단어를 팰린드롬(palindrome)이라고 합니다. 예를들어서 racecar, 10201은 팰린드롬 입니다.
    + 두 자연수 n, m이 매개변수로 주어질 때, n 이상 m 이하의 자연수 중 팰린드롬인 숫자의 개수를 return 하도록 solution 함수를 완성해 주세요.

<br/>

+ ⚠️제한사항
    + m은 500,000이하의 자연수이며, n은 m 이하의 자연수입니다.
    
<br/>

+ 📜입출력 예

   |    n    |    m   |   result   |
   | :-----: | :----: | :--------: |
   |    1    |   100  |     18     |
   |  100    |   300  |     20     |


   + 입출력 예 #1 : 1 이상 100 이하의 팰린드롬은 다음과 같이 18개가 있습니다.  
        1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 22, 33, 44, 55, 66, 77, 88, 99
   + 입출력 예 #2 : 100 이상 300 이하의 팰린드롬은 다음과 같이 20개가 있습니다.  
        101, 111, 121, 131, 141, 151, 161, 171, 181, 191, 202, 212, 222, 232, 242, 252, 262, 272, 282, 292

<br/>

+ ✏️코드

+ 내 코드

```cpp
    #include <string>
    #include <vector>
    #include <iostream>

    using namespace std;

    bool isPalindrome(int num);
    int solution(int n, int m) {
        int answer = 0;
        
        for(int i = n; i <= m; i++){
            if(isPalindrome(i))
                answer++;
        }
        return answer;
    }

    bool isPalindrome(int num){
        if(1 <= num && num <= 9) 
            return true;
        
        string s = to_string(num);
        
        for(int i = 0; i < (s.length() / 2) ; i++){
            if(s[i] != s[s.length()-1-i])
                return false;
        }
        
        return true;
        
    }
```

+ 교수님 코드

```cpp

```