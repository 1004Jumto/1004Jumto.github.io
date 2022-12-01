---
title: "[C++ Programming] 실습Part2: 행렬의 곱셈"
excerpt: "[C++ Programming] 13주차 2차시"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 행렬의 곱셈

+ 📝문제 설명 

    + 2차원 행렬 arr1과 arr2를 입력받아, arr1에 arr2를 곱한 결과를 반환하는 함수, solution을 완성해주세요.
	
<br/>

+ ⚠️제한사항
    + 행렬 arr1, arr2의 행과 열의 길이는 2 이상 100 이하입니다.
    + 행렬 arr1, arr2의 원소는 -10 이상 20 이하인 자연수입니다.
    + 곱할 수 있는 배열만 주어집니다.
    
<br/>

+ 📜입출력 예

   |  arr1        |       arr2      |   return  | 
   | :-----------: | :---------------: | :---: |
   | [[1, 4], [3, 2], [4, 1]]  |   [[3, 3], [3, 3]]   | [[15, 15], [15, 15], [15, 15]] |
   | [[2, 3, 2], [4, 2, 4], [3, 1, 4]]  |  [[5, 4, 3], [2, 4, 1], [3, 1, 1]]   | [[22, 22, 11], [36, 28, 18], [29, 20, 14]] |

<br/>

+ ✏️풀이

```cpp
    #include<iostream>
    #include<vector>

    using namespace std;

    class Matrix
    {
    public:
        Matrix(vector<vector<int>> v);
        Matrix operator*(Matrix& m);
        friend ostream& operator<<(ostream& os, const Matrix& m);

    private:
        vector<vector<int>> values;
        int getInnerProduct(Matrix& m, int i, int j);

    protected:

    };

    Matrix::Matrix(vector<vector<int>> v)
    {
        values = v;
    }

    Matrix Matrix::operator*(Matrix& m)
    {
        vector<vector<int>> v;
        
        for (int i = 0; i < values.size(); i++) {
            vector<int> row;
            for (int j = 0; j < m.values[0].size(); j++) {
                row.push_back(getInnerProduct(m, i, j));
            }
            v.push_back(row);
        }

        return Matrix(v);
    }

    int Matrix::getInnerProduct(Matrix& m, int i, int j)
    {
        int sum = 0;
        for (int k = 0; k < values[0].size(); k++) {
            sum += values[i][k] * m.values[k][j];
        }

        return sum;
    }

    ostream& operator<<(ostream& os, const Matrix& m) {
        for (int i = 0; i < m.values.size(); i++) {
            for (int j = 0; j < m.values[0].size(); j++) {
                os << m.values[i][j] << ' ';
            }
            os << endl;
        }
        return os;
    }

    int main(void) {
        vector<vector<int>> v, w;
        vector<int> row;
        
        row.push_back(1);
        row.push_back(2);
        row.push_back(3);
        v.push_back(row);
        row.clear();

        row.push_back(4);
        row.push_back(5);
        row.push_back(6);
        v.push_back(row);
        row.clear();

        row.push_back(1);
        row.push_back(2);
        w.push_back(row);
        row.clear();

        row.push_back(3);
        row.push_back(4);
        w.push_back(row);
        row.clear();

        row.push_back(5);
        row.push_back(6);
        w.push_back(row);
        row.clear();

        Matrix arr1(v);
        Matrix arr2(w);
        Matrix res = arr1 * arr2;
        cout << res << endl;

        return 0;
    }
```