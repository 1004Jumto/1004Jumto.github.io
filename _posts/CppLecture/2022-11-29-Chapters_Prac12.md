---
title: "[C++ Programming] 실습Part2: 2개 이하로 다른 비트"
excerpt: "[C++ Programming] 13주차 1차시"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 2개 이하로 다른 비트

+ 📝문제 설명 

    + 양의 정수 x에 대한 함수 f(x)를 다음과 같이 정의합니다.
    + x보다 크고 x와 비트가 1~2개 다른 수들 중에서 제일 작은 수 
    
    + 예를 들어,  
    f(2) = 3 입니다. 다음 표와 같이 2보다 큰 수들 중에서 비트가 다른 지점이 2개 이하이면서 제일 작은 수가 3이기 때문입니다.
        
   | 수 |	비트 |	다른 비트의 개수 |
   |:--:|:-----:|:----------------:|
   | 2	| 000...0010| |	
    | 3 | 	000...0011 | 	1 |  

    f(7) = 11 입니다. 다음 표와 같이 7보다 큰 수들 중에서 비트가 다른 지점이 2개 이하이면서 제일 작은 수가 11이기 때문입니다.
    
    |수 | 	비트 |	다른 비트의 개수 |
    |:--:|:----:|:----------------:|
    | 7 |	000...0111 | 	 |
    | 8 |	000...1000 | 	4 |
    | 9 |	000...1001 | 	3 |
    | 10 |	000...1010 | 	3 |
    | 11 |	000...1011 | 	2 |

    정수들이 담긴 배열 numbers가 매개변수로 주어집니다. numbers의 모든 수들에 대하여 각 수의 f 값을 배열에 차례대로 담아 return 하도록 solution 함수를 완성해주세요.

<br/>

+ ⚠️제한사항
    + 1 ≤ numbers의 길이 ≤ 100,000
    + 0 ≤ numbers의 모든 수 ≤ 1015
    
<br/>

+ 📜입출력 예

   |  numbers       |       result      | 
   | :-----------: | :---------------: | 
   | [2,7]  |   [3,11]   | 

   + 입출력 예 #1: 위의 예시와 같습니다.

<br/>

+ ✏️풀이

```cpp
    #include<iostream>
    #include<vector>

    using namespace std;

    class FX
    {
    public:
        FX(long long n);
        long long solution();

    protected:
        long long n;
        int diff(long long num, long long com);		// 다른 비트의 수를 리턴하는 함수
                                                    // protected혹은 private영역으로 선언해줌
        virtual int limit();						// 이 때 solution은 FX에만 있기 때문에 limit이 FX의 limit만 호출된다. 
                                                    // 이를 방지하기 위해 virtual을 선언해주어야 한다
    };

    FX::FX(long long n)
    {
        this->n = n;		// 원래 변수 이름 중복은 피하는게 맞음, 교수님이 this 쓰는거 보여주려고 일부로
    }

    long long FX::solution() {
        long long i;

        for (i = n + 1; diff(n, i) > limit(); i++) {
        }
        return i;
        //diff(n, i) > 2 를 고치면 내가 원하는 자리수 검사를 할 수 있는데,, 상속을 이용하는 것이 좋음
    }

    int FX::diff(long long num, long long com) {
        int count = 0;				// 다른 개수 카운트 하는 변수
        int len = sizeof(num) * 8;	// n이 몇 비트로 이루어져 있는지 구한 뒤 8을 곱해 맞춰줌
        long long mask = 1;

        for (int j = 0; j < len; j++) {
            // 모든 자리의 비트를 검사
            // n&mask, i&mask => 맨 마지막 비트가 뽑힌다고..? 비트 와이즈..?
            if ((num & mask) != (com & mask)) {
                count++;
            }
            mask = (mask << 1);		// 왼쪽으로 한번 옮김
        }

        return count;
    }

    int FX::limit() {
        return 2;
    }

    class FX2 : public FX {
    protected:
        int limit();

    public:
        FX2(long long n);
        //long long solution();	// override
        // 이때 solution함수는 숫자 하나 빼고 거의 겹친다. 중복을 최소화 시키는 것이 c++의 철학이므로 수정할 필요가 있다.
        // 허용하는 비트수를 만들어주는 함수를 하나 만든다.
    };

    FX2::FX2(long long n) : FX(n) { ; }

    int FX2::limit() {
        return 3;
    }	

    //long long FX2::solution() {
    //	long long i;
    //
    //	for (i = n + 1; diff(n, i) > 3; i++) {
    //		return i;
    //	}
    //}

    int main(void) {
        long long n = 7;

        FX fx(n);
        cout << n << " : " << fx.solution() << endl;

        FX2 fx2(n);
        cout << n << " : " << fx2.solution() << endl;
    }
```