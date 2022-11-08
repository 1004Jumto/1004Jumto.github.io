---
title: "[C++ Programming] 실습Part2 : 3진법 뒤집기"
excerpt: "[C++ Programming] 10주차 2차시"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 3진법 뒤집기

+ 📝문제 설명 

    + 자연수 n이 매개변수로 주어집니다.  
    n을 3진법 상에서 앞뒤로 뒤집은 후, 이를 다시 10진법으로 표현한 수를 return 하도록 solution 함수를 완성해주세요.

<br/>

+ ⚠️제한사항
    + n은 1 이상 100,000,000 이하인 자연수입니다.
    
<br/>

+ 📜입출력 예

   |            n             |       result      | 
   | :----------------------: | :---------------: | 
   |             45      |           7  |  
   |      125                |          229       |  

<br/>

+ ✏️코드

+ 내코드

```cpp
    #include <string>
    #include <vector>
    #include <iostream>

    using namespace std;

    string toThird(int n) {
        int p = 1;
        string res = "";

        while (n > 2) {
            res = (char)((n % 3) + '0') + res;
            p *= 10;
            n /= 3;
        }
        
        res = (char)((n % 3) + '0') + res;
        
        return res;
    }

    string reverseNum(string n) {
        string tmp = "";

        for (int i = 0; i < n.length(); i++) {
            char c = n[i];
            tmp = c + tmp;
        }

        return tmp;
    }

    int toDecimal(string n) {
        int res = 0;
        int p = 1;

        for (int i = n.length() - 1; i >= 0; i--, p *= 3) {
            res += (n[i] - '0')*p;
        }

        return res;
    }

    int solution(int n) {
        int answer = 0;
        string res;
        res = toThird(n);
        res = reverseNum(res);
        answer = toDecimal(res);

        return answer;
    }
```

+ 교수님 코드

```cpp
    #include<string>
    #include<iostream>
    #include<vector>

    using namespace std;

    class Number 
    {
    private:
        string n;   
        int k;      // 현재 진법
        void setNum(int n);

    public:
        Number(int n);
        void changeNum(int k);
        string getN();
        int getNum();
        void setNum(string n);
    };

    Number::Number(int n) 
    {
        this->k = 10;
        setNum(n);
    }

    //int -> string 변환 함수
    void Number::setNum(int n) 
    {
        this->n = "";
        for (int m = n; m > 0; m /= 10) {
            this->n = (char)('0' + (m % 10)) + this->n;
        }
    }

    // string을 10진수 숫자로 변환하는 함수
    int Number::getNum() 
    {
        int i = 1;
        int dec = 0;

        for (int m = n.length() - 1; m >= 0; m--) {
            dec += (n[m]-'0') * i;
            i *= k;
        }

        return dec;
    }

    // 원하는 진법으로 변환하는 함수
    void Number::changeNum(int k)
    {
        int dec = getNum();
        this->k = k;
        this->n = "";
        
        for (int m = dec; m > 0; m /= k) {
            this->n = (char)('0' + (m % k)) + this->n;
        }
    }

    string Number::getN()
    {
        return n;
    }

    void Number::setNum(string n)
    {
        this->n = n;
    }

    class Converter
    {
    public:
        static string reverse(string s);
    };

    string Converter::reverse(string s)
    {
        string ret = "";
        
        for (int i = 0; i < s.length(); i++) {
            ret = s[i] + ret;
        }

        return ret;
    }

    int main(void) {
        Number mynum(45);
        cout << mynum.getN() << endl;
        
        mynum.changeNum(3);
        cout << mynum.getN() << endl;
        mynum.setNum(Converter::reverse(mynum.getN()));
        cout << mynum.getN() << endl;

        //mynum.changeNum(10);
        //cout << mynum.getN() << endl;

        cout << mynum.getNum() << endl;

        return 0;
    }

```