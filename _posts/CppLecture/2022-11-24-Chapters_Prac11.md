---
title: "[C++ Programming] 실습Part2: 가장 큰 정사각형 찾기"
excerpt: "[C++ Programming] 12주차 2차시"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 가장 큰 정사각형 찾기

+ 📝문제 설명 

    + 1와 0로 채워진 표(board)가 있습니다. 표 1칸은 1 x 1 의 정사각형으로 이루어져 있습니다.  
    표에서 1로 이루어진 가장 큰 정사각형을 찾아 넓이를 return 하는 solution 함수를 완성해 주세요.  
    (단, 정사각형이란 축에 평행한 정사각형을 말합니다.)

    + 예를 들어  
    ```
    1	2	3	4
    0	1	1	1
    1	1	1	1
    1	1	1	1
    0	0	1	0
    ```  
    가 있다면 가장 큰 정사각형은  
    ```
    1	2	3	4
    0	1	1	1
    1	1	1	1
    1	1	1	1
    0	0	1	0
    ```  
    가 되며 넓이는 9가 되므로 9를 반환해 주면 됩니다.

<br/>

+ ⚠️제한사항
    + 표(board)는 2차원 배열로 주어집니다.
    + 표(board)의 행(row)의 크기 : 1,000 이하의 자연수
    + 표(board)의 열(column)의 크기 : 1,000 이하의 자연수
    + 표(board)의 값은 1또는 0으로만 이루어져 있습니다.
    
<br/>

+ 📜입출력 예

   |  board      |       answer      | 
   | :-----------: | :---------------: | 
   | [[0,1,1,1],[1,1,1,1],[1,1,1,1],[0,0,1,0]]   |   9   |  
   |   [[0,0,1,1],[1,1,1,1]]    |  	4   | 

   + 입출력 예 #1: 위의 예시와 같습니다.
   + 입출력 예 #2:   
    ```
    | 0 | 0 | 1 | 1 |
    | 1 | 1 | 1 | 1 |  
    ```  
    로 가장 큰 정사각형의 넓이는 4가 되므로 4를 return합니다.

<br/>

+ ✏️풀이

    ```cpp
    #include<iostream>
    #include<vector>
    #include<algorithm>

    using namespace std;

    class P11 {
    private:
        int width;
        int height;
        vector<vector<int>> table;
        int getArea(int top, int left);		// i,j가 왼쪽상단 점일때 가장 큰 정사각형 면적을 구하는 함수
        bool isSquare(int top, int left, int bottom, int right);

    public:
        P11(vector<vector<int>> table);
        int solution();		// 가장 큰 정사각형 면적 구하는 함수
    };


    P11::P11(vector<vector<int>> table)
    {
        this->table = table;
        width = table[0].size();	// 행 사이즈
        height = table.size();		// 열 사이즈
    }

    // 내부적으로 쓰고 싶은 함수이지 외부적으로 쓰려고 만든 함수가 아님.
    // 굳이 내 클래스 밖에서 이 함수를 알 필요는 없다
    // 따라서 Sub로 만들어진 함수들은 private으로 놓는 것이 정석
    int P11::getArea(int top, int left)
    {
        int maxarea = 0;
        for (int j = left; j < width; j++) {
            for (int i = top; i < height; i++) {
                if (isSquare(top, left, i, j)) {
                    int area = (i - top + 1) * (j - left + 1);
                    if (area > maxarea) {
                        maxarea = area;
                    }
                }
            }
        }

        return maxarea;
    }

    bool P11::isSquare(int top, int left, int bottom, int right)
    {
        // 가로와 세로가 똑같은지
        if ((bottom - top) != (right - left)) {
            return false;
        }
        
        // 가로와 세로가 같을 때 그 안이 모두 1로 채워져 있는지 검사
        for (int i = top; i <= bottom; i++) {
            for (int j = left; j <= right; j++) {
                if (table[i][j] != 1) {
                    return false;
                }
            }
        }

        return true;
    }

    int P11::solution()
    {
        int maxArea = 0;

        // 먼저 1로 이어진 선분을 찾고, 그 선분이 아래로 자라날 수 있는지 체크
        for (int i = 1; i < height; i++) {
            for (int j = 0; j < width; j++) {
                int area = getArea(i, j);
                if (area > maxArea) {
                    maxArea = area;
                }
            }
        }

        return maxArea;
    }

    int main(void) {
        vector<vector<int>> table = { {0,1,1,1}, {1,1,1,1}, {1,1,1,1}, {0,0,1,0} };
        
        P11 p11(table);
        cout << p11.solution() << endl;

        return 0;
    }
    ```
<br>

## 확장

```cpp
#include <iostream>
#include <vector>
using namespace std;

class P11
{
protected:
    vector<vector<int>> table;
    int width;
    int height;
    // virtual int getArea(int top, int left); 
    // P11의 solution()이 호출되는 순간 getArea()는 virtual 함수이므로 런타임에 getArea를 호출하는 객체의 getArea함수를 가져온다
    int getArea(int top, int left);
    // virtual bool isWhatIWant(int top, int left, int bottom, int right);
    bool isWhatIWant(int top, int left, int bottom, int right);
    virtual bool isCorrectSize(int top, int left, int bottom, int right); // 정사각형인지 직사각형인지 체크

public:
    P11(vector<vector<int>> table);
    int solution();
};
/* // 가장 넓은 직사각형 바꾸기 미션! */
/* P112 */
class P112 : public P11
{
    // getArea에서 issquare을 수정해야하므로 통채로 getArea를 오버라이드 해서 isRect이라는 함수를 새로 만든다
    // int getArea(int top, int left);
    // virtual bool isWhatIWant(int top, int left, int bottom, int right);
    virtual bool isCorrectSize(int top, int left, int bottom, int right);

public:
    P112(vector<vector<int>> table);
    // int solution();
};

////////// P11 ///////////
P11::P11(vector<vector<int>> table)
{
    this->table = table;
    this->width = table[0].size();
    this->height = table.size();
}
int P11::solution()
{
    int maxarea = 0;
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            int area = getArea(i, j); // 이 때의 getArea는 private으로 선언한다. 내부적으로만 쓰이기 때문에 굳이 외부적으로 드러내지 않는다
            if (area > maxarea)
            {
                maxarea = area;
            }
        }
    }
    cout << maxarea << endl;
    return maxarea;
}
int P11::getArea(int top, int left)
{
    int maxarea = 0;
    for (int j = left; j < width; j++)
    {
        for (int i = top; i < height; i++)
        {
            if (isWhatIWant(top, left, i, j))
            // top left bottom right
            {
                int area = (i - top + 1) * (j - left + 1);
                if (area > maxarea)
                {
                    maxarea = area;
                }
            }
        }
    }
    return maxarea;
}
bool P11::isWhatIWant(int top, int left, int bottom, int right)
{
    if (!isCorrectSize(top, left, bottom, right)) // 가로 세로 길이 다른 경우
        return false;
    for (int i = top; i <= bottom; i++)
    {
        for (int j = left; j <= right; j++)
        {
            if (table[i][j] != 1) // 그 영역이 모두 1이 아니면 무조건 false 반환
            {
                return false;
            }
        }
    }
    return true;
}
bool P11::isCorrectSize(int top, int left, int bottom, int right)
{
    if ((bottom - top) != (right - left))
        return false;
    return true;
}

///////////// P112 ////////////////
P112::P112(vector<vector<int>> table) : P11(table)
{;}
/*
int P112::solution()
{
    int maxarea = 0;
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            int area = getArea(i, j); 
            if (area > maxarea)
            {
                maxarea = area;
            }
        }
    }
    cout << maxarea << endl;
    return maxarea;
}
*/
/*
int P112::getArea(int top, int left)
{
    int maxarea = 0;
    for (int j = left; j < width; j++)
    {
        for (int i = top; i < height; i++)
        {
            if (isRect(top, left, i, j))
            // top left bottom right
            {
                int area = (i - top + 1) * (j - left + 1);
                if (area > maxarea)
                {
                    maxarea = area;
                }
            }
        }
    }
    return maxarea;
}
*/
/*
bool P112::isWhatIWant(int top, int left, int bottom, int right)
{
    for (int i = top; i <= bottom; i++)
    {
        for (int j = left; j <= right; j++)
        {
            if (table[i][j] != 1) // 그 영역이 모두 1이 아니면 무조건 false 반환
            {
                return false;
            }
        }
    }
    return true;
}
*/
bool P112::isCorrectSize(int top, int left, int bottom, int right)
{
    return true;
}
int main()
{
    P11 myp11({{1, 1, 1, 1}, {1, 1, 1, 1}, {1, 1, 1, 1}, {0, 0, 1, 0}});
    myp11.solution();

    P112 myp112({{1, 1, 1, 1}, {1, 1, 1, 1}, {1, 1, 1, 1}, {0, 0, 1, 0}});
    myp112.solution();
}
```