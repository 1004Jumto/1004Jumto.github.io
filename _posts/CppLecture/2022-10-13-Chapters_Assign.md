---
title: "[C++ Programming] 과제Part1 : 4-1.다항식 더하기, 4-2. 소인수분해"
excerpt: "[C++ Programming] 과제 4-1, 4-2"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 💻과제

### 4-1. 다항식 더하기

+ 📝문제 설명 

    한 개 이상의 항의 합으로 이루어진 식을 다항식이라고 합니다.  
    다항식을 계산할 때는 동류항끼리 계산해 정리합니다.  
    덧셈으로 이루어진 다항식 polynomial이 매개변수로 주어질 때, 동류항끼리 더한 결괏값을 문자열로 return 하도록 solution 함수를 완성해보세요.  
    같은 식이라면 가장 짧은 수식을 return 합니다.  


+ ⚠️제한사항
    + 0 < polynomial에 있는 수 < 100
    + polynomial에 변수는 'x'만 존재합니다.
    + polynomial은 0부터 9까지의 정수, 공백, ‘x’, ‘+'로 이루어져 있습니다.
    + 항과 연산기호 사이에는 항상 공백이 존재합니다.
    + 공백은 연속되지 않으며 시작이나 끝에는 공백이 없습니다.
    + 하나의 항에서 변수가 숫자 앞에 오는 경우는 없습니다.
    + " + 3xx + + x7 + "와 같은 잘못된 입력은 주어지지 않습니다.
    + "012x + 001"처럼 0을 제외하고는 0으로 시작하는 수는 없습니다.
    + 문자와 숫자 사이의 곱하기는 생략합니다.
    + polynomial에는 일차 항과 상수항만 존재합니다.
    + 계수 1은 생략합니다.
    + 결괏값에 상수항은 마지막에 둡니다.
    + 0 < polynomial의 길이 < 50

+ 입출력 예시

    + 입력 | "3x + 7 + x"	
    + 출력 | "4x + 7"	


+ ✏️코드

```cpp

///////////////////////// My Solution //////////////////////////////
    #include <string>
    #include <algorithm>
    #include <vector>

    using namespace std;

    string solution(string polynomial) {

        string answer = "";


        // 1. "+" 기준으로 분류
        int idxOld = 0;
        int idxNew = 0;
        vector<string> items;
        string op = "+";

        while (1) {
            idxNew = polynomial.find(op, idxOld);
            
            if (idxNew == -1) {
                items.push_back(polynomial.substr(idxOld));
                break;
            }

            else {
                items.push_back(polynomial.substr(idxOld, idxNew - idxOld));
                idxOld = idxNew + 1;
            }
        }

        // 2. 동류항끼리 묶기
        vector<int> itemX;
        vector<int> itemN;
        idxOld = 0, idxNew = 0;

        for (int i = 0; i < items.size(); i++) {
            idxNew = items[i].find("x");
            
            if (idxNew >= 0) {
                items[i].erase(idxNew, 1);
                
                // 앞에 계수가 1인 경우도 고려,,
                try {
                    itemX.push_back(stoi(items[i]));
                }
                catch(...) {
                    itemX.push_back(1);
                }
            }
            else {
                itemN.push_back(stoi(items[i]));
            }
        }

        // 3. 계산
        int sumX = 0;
        int sumN = 0;

        for (int i : itemX) {
            sumX += i;
        }

        for (int i : itemN) {
            sumN += i;
        }

        // 4. 출력
        if (sumX == 0)
            answer = to_string(sumN);

        else if (sumX == 1 && sumN == 0)
            answer = "x";

        else if (sumX != 1 && sumN == 0)
            answer = to_string(sumX) + "x";

        else if (sumX == 1 && sumN != 0)
            answer = "x + " + to_string(sumN);

        else
            answer = to_string(sumX) + "x + " + to_string(sumN);

        return answer;
    }

///////////////////////// Professor Solution //////////////////////////////
    int myStoi(string num){
        int digit = 1;
        int n = 0;

        // 1의 자리부터 숫자로 바꿔 더해준다
        for(int i = num.length()-1; i >= 0; i--, digit *= 10){   
            n += (num[i] - '0') * digit;
        }

        return n;
    }

    string myItos(int n){
        string num = "";

        for(int i = n; i > 0; i /= 10){
            char tmp = (i % 10) + '0';
            num = tmp + num;
        }

        return num;
    }

    vector<string> tokenize(string letter){
        vector<string> tokens;
        int i, j;

        for(i = 0; i < letter.length(); i = j + 1){
            for(j = i; j < letter.length(); j++){
                if(letter[j] == ' '){
                    tokens.push_back(letter.substr(i, j-i));
                    break;
                }
            }
            if(j == letter.length()){
                tokens.push_back(letter.substr(i, j-i+1));
            }
        }
    }

    string solution(string polynomial){
        string answer = "";
        int cof = 0;
        int constant = 0;
        
        vector<string> tokens = tokenize(polynomial);   // 토큰으로 나눠서 vector에 넣어주는 함수

        // 일반 상수, x의 계수, + 세가지로 구분가능. +는 무시해도 되고 상수와 계수만 모으면 됨
        
        for(int i = 0; i < tokens.size(); i++){
            if(tokens[i] == "+"){}
            
            else if(tokens[i][tokens[i].length()-1] == 'x'){
                if(tokens[i].length() == 1){        // X의 계수가 1인 경우
                    cof++;
                }
                else{
                    cof += myStoi(tokens[i].substr(0, tokens[i].length()-1));
                }
            }
            else{
                constant += myStoi(tokens[i]);
            }
        }

        if(cof == 0){
            answer = myItos(constant);
        }
        else{
            if(cof == 1){
                answer = "x"; 
            }
            else{
                answer = myItos(cof) + "x";
            }
            if(constant > 0){
                answer = myItos(cof) + "x + " + myStoi(constant);
            }
        }
        
        return answer;
    }
```

<br/>

### 4-2. 소인수분해

+ 📝문제 설명 

    소인수분해란 어떤 수를 소수들의 곱으로 표현하는 것입니다.  
    예를 들어 12를 소인수 분해하면 2 * 2 * 3 으로 나타낼 수 있습니다. 따라서 12의 소인수는 2와 3입니다.  
    자연수 n이 매개변수로 주어질 때 n의 소인수를 오름차순으로 담은 배열을 return하도록 solution 함수를 완성해주세요.  

+ ⚠️제한사항
    + 2 ≤ n ≤ 10,000

+ ✏️코드

```cpp
    #include <string>
    #include <vector>
    #include <algorithm>

    using namespace std;

    vector<int> solution(int n) {
        vector<int> yaksu;

        for(int i=2; i<=n; ){
            if(n%i == 0){
                yaksu.push_back(i);
                n /= i;
                i = 2;
            }
            
            else
                i++;
        }   
        
        // 중복 제거
        yaksu.erase(unique(yaksu.begin(), yaksu.end()), yaksu.end());
        
        return yaksu;
    }
```