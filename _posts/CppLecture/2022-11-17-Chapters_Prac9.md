---
title: "[C++ Programming] 실습Part2 : 핸드폰 번호 가리기"
excerpt: "[C++ Programming] 12주차 1차시: "
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 핸드폰 번호 가리기

+ 📝문제 설명 

    + 프로그래머스 모바일은 개인정보 보호를 위해 고지서를 보낼 때 고객들의 전화번호의 일부를 가립니다.  
    전화번호가 문자열 phone_number로 주어졌을 때, 전화번호의 뒷 4자리를 제외한 나머지 숫자를 전부 *으로 가린 문자열을 리턴하는 함수, solution을 완성해주세요.

<br/>

+ ⚠️제한사항
    + phone_number는 길이 4 이상, 20이하인 문자열입니다.
    
<br/>

+ 📜입출력 예

   |   phone_number        |       result      | 
   | :----------------------: | :---------------: | 
   |   "01033334444"   |   "*******4444"   |  
   |   "027778888"    |   "*****8888"    | 
  

<br/>

+ ✏️코드

    + 내 코드

    ```cpp
        #include <string>
        #include <vector>

        using namespace std;

        string solution(string phone_number) {
            string answer = "";
            
            for(int i=0;i<phone_number.length()-4;i++){
                answer += "*";
            }
            for(int i=phone_number.length()-4; i<phone_number.length();i++){
                answer += phone_number[i];
            }
            
            return answer;
        }
    ```

    + 교수님 코드

    ```cpp
    #include<iostream>

    using namespace std;

    class PhoneNumber
    {
    public:
        PhoneNumber(string num);
        ~PhoneNumber();
        string masking(int k) const;
        friend ostream& operator<<(ostream& os, const PhoneNumber& p);	
    private:
        string number;
    };

    PhoneNumber::PhoneNumber(string num)
    {
        number = num;
    }

    PhoneNumber::~PhoneNumber()
    {
    }

    string PhoneNumber::masking(int k) const{
        string masked = "";
        for (int i = 0; i < number.length(); i++) {
            if (i < k) {
                masked += "*";
            }
            else {
                masked += number[i];
            }
        }
        return masked;
    }

    ostream& operator<<(ostream& os, const PhoneNumber& p) {
        string masked = p.masking(7); // const로 묶었기 때문에 p에 대한 변경을 허용하지 않는다
                                    // 따라서 p가 변할 수도 있는 멤버함수를 호출하려면 멤버함수도 const선언된 함수여야 함
        os << masked;
        return os;
    }

    int main(void) {
        PhoneNumber number("01073489592");
        cout << number.masking(7);
        return 0;
    }
    ```

<br>
    
## 클래스 구현 연습문제1

+ 복소수 클래스를 만들어 연산자 오버로딩을 해보자

```cpp
    #include<iostream>
    #include<string>

    using namespace std;

    class Complex
    {
    public:
        Complex(int a, int b);
        ~Complex();
        string get() const;
        friend Complex operator+(const Complex& x, const Complex& y);
        friend ostream& operator<<(ostream& os, const Complex& num);
    private:
        int real;
        int img;
    };

    Complex::Complex(int a, int b) : real(a), img(b)
    {
    }

    Complex::~Complex()
    {
    }

    string Complex::get() const
    {
        string s;

        if (img > 0) {
            s = to_string(real) + "+" + to_string(img) + "i";
        }
        else if (img < 0) {
            s = to_string(real) + to_string(img) + "i";
        }
        else {
            s = to_string(real);
        }

        return s;
    }

    Complex operator+(const Complex& x, const Complex& y)
    {
        Complex answer(x.real + y.real, x.img + y.img);
        return answer;
    }

    ostream& operator<<(ostream& os, const Complex& num)
    {
        if (num.real != 0) {
            os << num.real;
        }

        if (num.img < 0) {
            os << " - " << -num.img << "i";
        }
        else if (num.img > 0) {
            os << " + " << num.img << "i";
        }

        return os;
    }

    int main(void) {
        Complex com(4, -3);
        Complex com1(5, -3);
        
        cout << (com + com1);

        return 0;
    }
```

<br>

## 클래스 구현 연습하기2

+ 2차원 좌표 클래스를 구현해보자. 
+ length는 벡터의 길이를 리턴한다
+ operator*는 내적값을 리턴한다

```cpp
    #include<iostream>
    #include<string>
    #include<cmath>

    using namespace std;

    class Point
    {
    public:
        Point();
        Point(float xpos, float ypos);
        ~Point();
        float getLength();
        float operator*(Point& pos);		// friend이면 파라미터가 두개이고 멤버함수 아니다
                                            // 멤버함수로 만들면 파라미터 하나만 받아도 됨
        friend ostream& operator<<(ostream& os, Point& pos);

    private:
        float x;
        float y;
    };

    Point::Point(): x(0), y(0)
    {
    }

    Point::Point(float xpos, float ypos): x(xpos), y(ypos)
    {
    }

    Point::~Point()
    {
    }

    float Point::operator*(Point& pos)
    {
        float ans = (x * pos.x) + (y * pos.y);
        return ans;
    }

    float Point::getLength()
    {
        return sqrt(x * x + y * y);
    }

    ostream& operator<<(ostream& os, Point& pos)
    {
        os << "(" << pos.x << ", " << pos.y << ")";

        return os;
    }


    int main(void) {
        Point pos1(2, 5);
        Point pos2(3, -4);
        
        cout << pos2 << endl;
        cout << pos2.getLength() << endl;	// 5가 나와야함
        cout << (pos1 * pos2) << endl;

        return 0;
    }
```

<br>

## 클래스 상속 구현 연습하기3 

+ 위의 Point클래스를 상속받는 3차원 Point클래스를 구현해보자

```cpp
    #include<iostream>
    #include<string>
    #include<cmath>

    using namespace std;

    class Point
    {
    public:
        Point();
        Point(float xpos, float ypos);
        ~Point();
        float getLength();
        float operator*(Point& pos);		// friend이면 파라미터가 두개이고 멤버함수 아니다
                                            // 멤버함수로 만들면 파라미터 하나만 받아도 됨
        friend ostream& operator<<(ostream& os, Point& pos);

    protected:			// 상속해주어야 하므로 protected로 선언함
                        // private은 상속받는 애들이 아예 접근하지 못하므로 protected로 바꿔주어야 유도클래스가 접근할 수 있다
        float x;
        float y;
    };

    Point::Point(): x(0), y(0)
    {
    }

    Point::Point(float xpos, float ypos): x(xpos), y(ypos)
    {
    }

    Point::~Point()
    {
    }

    float Point::operator*(Point& pos)
    {
        float ans = (x * pos.x) + (y * pos.y);
        return ans;
    }

    float Point::getLength()
    {
        return sqrt(x * x + y * y);
    }

    ostream& operator<<(ostream& os, Point& pos)
    {
        os << "(" << pos.x << ", " << pos.y << ")";

        return os;
    }

    class ThreeDiPoint : protected Point
    {
    public:
        ThreeDiPoint();
        ThreeDiPoint(float x, float y, float z);
        ~ThreeDiPoint();
        float getLength();
        float operator*(ThreeDiPoint& pos);
        friend ostream& operator<<(ostream& os, const ThreeDiPoint& pos);
    private:
        float z;
    };

    ThreeDiPoint::ThreeDiPoint() : z(0)
    {
    }

    ThreeDiPoint::ThreeDiPoint(float x, float y, float z) : Point(x, y), z(z) 
    {
    }

    ThreeDiPoint::~ThreeDiPoint()
    {
    }

    float ThreeDiPoint::getLength() {
        return sqrt(Point::getLength() * Point::getLength() + z * z);
    }

    float ThreeDiPoint::operator*(ThreeDiPoint& pos) {
        return (x * pos.x + y * pos.y + z * pos.z);
    }

    ostream& operator<<(ostream& os, const ThreeDiPoint& pos) {
        os << "(" << pos.x << ", " << pos.y << ", " << pos.z << ")";

        return os;
    }

    int main(void) {
        ThreeDiPoint pos1(2, 5, 1);
        ThreeDiPoint pos2(3, -4, 5);

        cout << pos2 << endl;
        cout << pos2.getLength() << endl;
        cout << (pos1 * pos2) << endl;

        return 0;
    }

```