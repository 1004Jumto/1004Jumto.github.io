---
title: "[C++ Programming] 실습Part2 : 문자열 다루기 기본 (feat.string 클래스)"
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
    #include <string>
    #include <vector>
    #include <iostream>

    using namespace std;

    ///////////////////////////////////////////////////////////////////////////
    /////////////     기본적인 문자열에 대한 클래스 구현해보기			/////////
    ///////////////////////////////////////////////////////////////////////////
    class MyString
    {
    protected:	// private으로 선언한다면 자식클래스가 pstr에 접근할 수 없기 때문에 protected로 바꿈
        char* pstr;
        void initPstr();

    public:
        MyString();
        ~MyString();
        int getLength();
        void setString(const char* t);
        char* getPstr();
        ostream& operator<<(ostream& os);
        friend ostream& operator<<(ostream& os, const MyString& s);
    };

    ////////////////////////// <summary> //////////////////////////////////////
    ////////////          MyString을 상속받은 문제를 풀기위한 클래스		////
    ////////////		  solve라는 함수를 추가함						///////
    ////////////////////////// </summary> /////////////////////////////////////
    class FiveString : public MyString {
    public:
        bool solve();
    };


    ////////////////////////// <summary> //////////////////////////////////////
    ////////////          string을 상속받은 클래스						////////
    ////////////////////////// </summary> /////////////////////////////////////
    class StringfromString : public string {
    public:
        StringfromString(const char* s);
        bool solve();
    };

    ////////////////////////// <summary> //////////////////////////////////////
    ////////////					Main 함수						/////////// 
    ////////////////////////// </summary> /////////////////////////////////////
    int main(void) {
        MyString mystr;
        mystr.setString("test");
        cout << mystr.getLength() << " : " << mystr.getPstr() << endl;
        cout << mystr.getLength() << " : " << mystr << endl;

        FiveString str;
        str.setString("1234");
        cout << str.getLength() << " : " << str << " : " << str.solve() << endl;

        StringfromString sfs("12ab56");
        cout << sfs.length() << " : " << sfs << " : " << sfs.solve() << endl;
        
        // 자식클래스는 부모클래스인 척 할 수 있다
        // MyString tmp = new FiveString();  (X)
        // FiveString tmp = new MyString();	 (O)
        
        return 0;
    }


    ///////////////////////////// MyString 멤버함수 //////////////////////////////
    MyString::MyString()
    {
        pstr = NULL;
        initPstr();
    }

    MyString::~MyString()
    {
        if (pstr != NULL) {
            delete[] pstr;
        }
    }

    void MyString::initPstr()

    {
        pstr = new char[10];
    }

    int MyString::getLength()
    {
        int i;
        
        for (i = 0; i < 10; i++) {
            if (pstr[i] == '\0')
                break;
        }

        return i;
    }

    void MyString::setString(const char* t)
    {
        for (int i = 0; i < 10; i++) {
            pstr[i] = t[i];
            if (t[i] == NULL) {
                break;
            }
        }
    }

    ostream& MyString::operator<<(ostream& os)
    {
        os << pstr;
        return os;
    }

    ostream& operator<<(ostream& os, const MyString& s)
    {
        os << s.pstr;
        return os;
    }

    char* MyString::getPstr()
    {
        return this->pstr;
    }

    ///////////////////////////// FiveString 멤버함수 //////////////////////////////
    bool FiveString::solve()
    {
        int len = getLength();

        if (len == 4 || len == 6) {
            for (int i = 0; i < 10 && pstr[i] != '\0'; i++) {
                if (pstr[i] < '0' || pstr[i]>'9') {
                    return false;
                }
            }
        }
        else {
            return false;
        }

        return true;
    }

    ///////////////////////////// StringfromString 멤버함수 ////////////////////////////////
    StringfromString::StringfromString(const char* s) : string(s){}	// 이니셜라이저 
                                                // 부모클래스를 명시적으로 호출, 부모클래스인 string클래스의 생성자가 호출된다
                                                // 자식클래스의 인스턴스가 생성될 때, 부모클래스의 인스턴스가 먼저 생성된다
    bool StringfromString::solve()
    {
        int len = this->length();

        if (len == 4 || len == 6) {
            for (int i = 0; i < 10 && (*this)[i] != '\0'; i++) {
                if ((*this)[i] < '0' || (*this)[i]>'9') {
                    return false;
                }
            }
        }
        else {
            return false;
        }

        return true;
    }
```  
+ 상속이 짱이다