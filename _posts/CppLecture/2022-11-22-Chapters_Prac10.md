---
title: "[C++ Programming] 실습Part2 : 소수찾기, 최대공약수와 최소공배수, 약수의 합"
excerpt: "[C++ Programming] 12주차 2차시: "
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 소수찾기

### 📝문제 설명 

1부터 입력받은 숫자 n 사이에 있는 소수의 개수를 반환하는 함수, solution을 만들어 보세요.  
소수는 1과 자기 자신으로만 나누어지는 수를 의미합니다.  
(1은 소수가 아닙니다.)

<br/>

### ⚠️제한사항

n은 2이상 1000000이하의 자연수입니다.
    
<br/>

### 📜입출력 예

   |   n       |       result      | 
   | :----------------------: | :---------------: | 
   |   10   |   4   |  
   |   5    |   3    | 

   + 입출력 예 #1: 1부터 10 사이의 소수는 [2,3,5,7] 4개가 존재하므로 4를 반환
   + 입출력 예 #2: 1부터 5 사이의 소수는 [2,3,5] 3개가 존재하므로 3를 반환

<br/>

### ✏️코드

+ 내 코드
    
```cpp
    #include <string>
    #include <vector>

    using namespace std;

    int solution(int n) {
        int* num = new int[n + 1];
                
        //배열 초기화
        for (int i = 0; i <= n; i++) {
            num[i] = i;
        }

        //2부터 배수를 지워준다
        for (int i = 2; i <= n; i++)
        {
            //num[i] 가 0이면 이미 소수가 아니므로 continue
            if (num[i] == 0)
                continue;

            for (int j = 2 * i; j <= n; j += i)
                num[j] = 0;
        }

        //1은 소수가 아니므로 예외처리
        num[1] = 0;
        
        int answer = 0;
        
        for(int i=2;i<=n;i++){
            if(num[i] != 0){
                answer++;
            }
        
        }
        return answer;
    }
```

+ 클래스와 상속을 이용한 코드

```cpp
    /*	8. 문제 설명

        1부터 입력받은 숫자 n 사이에 있는 소수의 개수를 반환하는 함수, solution을 만들어 보세요.
        소수는 1과 자기 자신으로만 나누어지는 수를 의미합니다.
        (1은 소수가 아닙니다.)
    */

    #include<iostream>
    #include<vector>
    #include<string>
    #include<algorithm>

    using namespace std;

    class Util {
    private:
        static bool isDiv(int a, int b);
    public:
        static bool isPrime(int n);
    };

    bool Util::isPrime(int n) {
        int i;
        for (i = 2; i <= n; i++) {
            if (isDiv(i, n)) {
                break;
            }
        }
        if (i == n) {
            return true;
        }
        else {
            return false;
        }
    }

    bool Util::isDiv(int a, int b) {
        return (b % a == 0);
    }

    class P {
    protected:
        int num;

    public:
        P(int n);
        virtual int solution() = 0;
    };

    P::P(int n) :num(n) { ; }

    class P8 :public P {
    private:

    public:
        P8(int n);
        int solution();     // 소수 개수 찾아야함
    };

    P8::P8(int n) :P(n) { ; }

    int P8::solution() {
        int res = 0;

        for (int i = 2; i <= num; i++) {
            if (Util::isPrime(i)) {
                res++;
            }
        }

        return res;
    }

    int main(void) {
        P* p = new P8(5);
        int res = p->solution();

        cout << res;

        return 0;
    }
```

<br>


## 최대공약수와 최소공배수

### 📝문제 설명 

두 수를 입력받아 두 수의 최대공약수와 최소공배수를 반환하는 함수, solution을 완성해 보세요.  
배열의 맨 앞에 최대공약수, 그다음 최소공배수를 넣어 반환하면 됩니다.  
예를 들어 두 수 3, 12의 최대공약수는 3, 최소공배수는 12이므로 solution(3, 12)는 [3, 12]를 반환해야 합니다.

<br/>

### ⚠️제한사항

두 수는 1이상 1000000이하의 자연수입니다.
    
<br/>

### 📜입출력 예

   |   n       |   m   |      result      | 
   | :-------: | :---: | :---------------: | 
   |   3    |   12   |   [3,12] |  
   |   2    |   5   |   [1,10] |

   + 입출력 예 #1: 위의 설명과 같습니다.
   + 입출력 예 #2: 자연수 2와 5의 최대공약수는 1, 최소공배수는 10이므로 [1, 10]을 리턴해야 합니다.

<br/>

### ✏️코드

+ 내 코드
    
```cpp
    #include <string>
    #include <vector>

    using namespace std;
    int getGCD(int n1, int n2){
            int c;

            while(n2 != 0){
                c = n1 % n2;
                n1 = n2;
                n2 = c;
            }

            return n1;
    }

    int getLCM(int n1, int n2){
            return n1 * n2 / getGCD(n1, n2);
        }

    vector<int> solution(int n, int m) {
        vector<int> answer;
        answer.push_back(getGCD(n,m));
        answer.push_back(getLCM(n,m));
        return answer;
    }
```

+ 클래스 P를 상속받은 코드

```cpp
    /*	9. 문제 설명

        두 수를 입력받아 두 수의 최대공약수와 최소공배수를 반환하는 함수, solution을 완성해 보세요.
        배열의 맨 앞에 최대공약수, 그다음 최소공배수를 넣어 반환하면 됩니다.
        예를 들어 두 수 3, 12의 최대공약수는 3, 최소공배수는 12이므로 solution(3, 12)는 [3, 12]를 반환해야 합니다.
    */

    #include<iostream>
    #include<vector>
    #include<string>
    #include<algorithm>

    using namespace std;

    class Util {
    private:
        static bool isDiv(int a, int b);
    public:
        static bool isPrime(int n);
    };

    bool Util::isPrime(int n) {
        int i;
        for (i = 2; i <= n; i++) {
            if (isDiv(i, n)) {
                break;
            }
        }
        if (i == n) {
            return true;
        }
        else {
            return false;
        }
    }

    bool Util::isDiv(int a, int b) {
        return (b % a == 0);
    }

    class P {
    protected:
        int num;

    public:
        P(int n);
        virtual int solution() = 0;
    };

    P::P(int n) :num(n) { ; }

    class P9 : public P {
    private:
        int n2;
        int getLCM();   // 최소공배수
        int getGCD();   // 최대공약수

    public:
        P9(int a, int b);
        int solution();
    };

    int P9::getLCM()
    {
        return num * n2 / (getGCD());
    }

    int P9::getGCD()
    {
        int res;
        int a = num, b = n2;

        while (b != 0) {
            res = a % b;
            a = b;
            b = res;
        }

        return a;
    }

    P9::P9(int a, int b) :P(a), n2(b) { ; }

    int P9::solution()
    {
        int gcd = getGCD();
        int lcm = getLCM();

        cout << gcd << "\n" << lcm;

        return 0;
    }

    int main(void) {
        P* p = new P9(3, 12);
        p->solution();

        return 0;
    }
```

<br>

## 약수의 합

### 📝문제 설명 

 정수 n을 입력받아 n의 약수를 모두 더한 값을 리턴하는 함수, solution을 완성해주세요.

<br/>

### ⚠️제한사항

n은 0 이상 3000이하인 정수입니다.
    
<br/>

### 📜입출력 예


   |   n       |       result      | 
   | :-------: | :---------------: | 
   |   12      |        28         |  
   |   5       |        6          | 

   + 입출력 예 #1: 12의 약수는 1, 2, 3, 4, 6, 12입니다. 이를 모두 더하면 28입니다.
   + 입출력 예 #2: 5의 약수는 1, 5입니다. 이를 모두 더하면 6입니다.

<br/>

### ✏️코드

+ 내 코드
    
```cpp
    #include <string>
    #include <vector>

    using namespace std;

    int solution(int n) {
        int answer = 0;

        for(int i=1;i<=n;i++){
            if(n%i==0)
                answer+=i;
        }
            
        return answer;
    }
```

+ 교수님 코드   

```cpp
    /* 10. 문제 설명

    정수 n을 입력받아 n의 약수를 모두 더한 값을 리턴하는 함수, solution을 완성해주세요.
    */

    #include<iostream>
    #include<vector>
    #include<string>
    #include<algorithm>

    using namespace std;

    class Util {
    private:
        static bool isDiv(int a, int b);
    public:
        static bool isPrime(int n);
    };

    bool Util::isPrime(int n) {
        int i;
        for (i = 2; i <= n; i++) {
            if (isDiv(i, n)) {
                break;
            }
        }
        if (i == n) {
            return true;
        }
        else {
            return false;
        }
    }

    bool Util::isDiv(int a, int b) {
        return (b % a == 0);
    }

    class P {
    protected:
        int num;

    public:
        P(int n);
        virtual int solution() = 0;
    };

    P::P(int n) :num(n) { ; }

    class P10 :public P {
    private:

    public:
        P10(int n);
        int solution();
    };

    P10::P10(int n) :P(n) { ; }

    // 약수 합 구하기
    int P10::solution()
    {
        int count = 0;
        for (int i = 0; i <= num; i++) {
            if (isDiv(i, num)) {
                count += i;
            }
        }

        return count;
    }

    int main(void) {
        P* p = new P10(10);

        cout << p->solution();

        return 0;
    }
```