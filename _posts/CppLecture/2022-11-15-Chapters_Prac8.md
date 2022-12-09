---
title: "[C++ Programming] 실습Part2 : 직사각형 별찍기 (feat. 추상클래스)"
excerpt: "[C++ Programming] 11주차 2차시"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 직사각형 별찍기

+ 📝문제 설명 

    + 이 문제에는 표준 입력으로 두 개의 정수 n과 m이 주어집니다.  
    별(*) 문자를 이용해 가로의 길이가 n, 세로의 길이가 m인 직사각형 형태를 출력해보세요.

<br/>

+ ⚠️제한사항
    + n과 m은 각각 1000 이하인 자연수입니다.
    
<br/>

+ 📜입출력 예

   ```
   |            입력             |       출력       |
    
                5 3                     *****       
                                        *****
                                        *****
   ```
  

<br/>

+ ✏️코드

```cpp
    #include <iostream>

    using namespace std;

    int main(void) {
        int a;
        int b;
        cin >> a >> b;
        
        for(int i = 0; i < b; i++){
            for(int j = 0; j < a; j++){
                cout << "*";
            }
            cout << "\n";
        }
        return 0;
    }
```
참 쉽죠?

+ 교수님 코드

```cpp
    #include<iostream>
    using namespace std;

    class Polygon
    {
    public:
        Polygon(int m, int n);
        ~Polygon();
        void draw();
    private:
    protected:
        int n, m;

    };

    Polygon::Polygon(int m, int n)
    {
        this->m = m;
        this->n = n;
    }

    Polygon::~Polygon()
    {
    }

    void Polygon::draw()
    {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                cout << "*";
            }
            cout << endl;
        }
    }

    int main(void) {
        int a, b;
        cin >> a >> b;
        Polygon polygon(a, b);
        polygon.draw();

        return 0;
    }
```  
  + 이렇게 클래스를 만들어 코딩했을 때의 장점  
    + 확장성!
    + 나의 코드처럼 메인함수에 바로 반복문을 사용하여 구현하거나, 함수를 통해 구현했다면 코드 줄 수는 적고 더 간단해보일 수 있지만, 확장하는 데에는 번거롭다
    + 그리는 방법을 클래스로 체계적으로 만들어 놓으면 확장성이 높아진다
    + **`void draw() = 0;`** 이라는 표시는 draw()라는 함수를 정의(구현)하지 않는다는 뜻을 나타내는 명시적 표현이다
        + 굳이 이렇게 구현하지 않을 것을 표현해놓는 이유는 무엇일까?
        ```cpp
            void Polygon::draw() = 0;   // 코드를 실행했을 때, polygon의 draw에 에러가 뜬다. 
            
            class Rect : public Polygon{
                public: 
                    Rect(int m, int n);
                    void draw();
            };

            Rect::Rect(int m, int n) : Polygon(m, n){}

            Rect::draw(){
                for (int i = 0; i < n; i++) {
                   for (int j = 0; j < m; j++) {
                        cout << "*";
                    }
                    cout << endl;
                }
            }

            class Triangle : public Polygon{
                public:
                    Triangle(int m, int n);
                    void draw();
            };

            Triangle::Triangle(int m, int n): Polygon(m, n){}

            Triangle::draw(){
                for (int i = 0; i < n; i++) {
                   for (int j = 0; j < i; j++) {
                        cout << "*";
                    }
                    cout << endl;
                }
            }

            int main(void){
                int a, b;
                cin >> a >> b;
                Rect rect(a, b);
                rect.draw();

                Polygon pol(a,b);   // error
                pol.draw();

                return 0;
            }
        ```
        + 위와 같이 draw라는 <u>함수는 존재하지만 정의되어있지 않은 추상함수를 가지고 있는 클래스를 <strong>추상클래스</strong>라고 한다</u>

<br>

## 추상클래스

+ abstract 함수를 가진 클래스

+ **인스턴스를 만들 수 없다**

+  클래스의 설계에서는 쓸모없어 보이더라도 추상 클래스의 설계가 더 좋은 것이다  
    자기가 그것을 구현하여 인스턴스를 만들고자 하는 것이 목적이 아닌, 훗날 유도클래스의 의무와 의도를 부여하는 클래스이다  
    <u>추상클래스의 가상함수를 오버라이드 하지 않는 유도클래스는 여전히 추상클래스이다</u>  
    따라서 이러한 가상함수는 자신을 상속받는 클래스에게 구현해야할 함수를 의무적으로 남기는 장치가 된다

+ 자바에서는 인터페이스와 추상 클래스는 존재 목적이 다릅니다.  
추상 클래스는 그 추상 클래스를 상속받아서 기능을 이용하고, 확장시키는 데 있습니다.  
반면에 인터페이스는 함수의 껍데기만 있는데, 그 이유는 그 함수의 구현을 강제하기 위해서 입니다  
<small>(출처 - 브런치https://brunch.co.kr)</small>
