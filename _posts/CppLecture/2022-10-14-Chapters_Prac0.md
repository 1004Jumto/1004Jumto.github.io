---
title: "[C++ Programming] 실습Part1 : 15.모스부호(1), 16.순서쌍 개수 구하기"
excerpt: "[C++ Programming] 6주차 2차시"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 💻실습

### 15. 모스부호(1)
  
+ 문제 설명 

머쓱이는 친구에게 모스부호를 이용한 편지를 받았습니다.  
그냥은 읽을 수 없어 이를 해독하는 프로그램을 만들려고 합니다.  
문자열 letter가 매개변수로 주어질 때, letter를 영어 소문자로 바꾼 문자열을 return 하도록 solution 함수를 완성해보세요.  
모스부호는 다음과 같습니다.

```
    morse = { 
        '.-':'a','-...':'b','-.-.':'c','-..':'d','.':'e','..-.':'f',
        '--.':'g','....':'h','..':'i','.---':'j','-.-':'k','.-..':'l',
        '--':'m','-.':'n','---':'o','.--.':'p','--.-':'q','.-.':'r',
        '...':'s','-':'t','..-':'u','...-':'v','.--':'w','-..-':'x',
        '-.--':'y','--..':'z'
    }
```

+ ⚠️제한사항
    + 1 ≤ letter의 길이 ≤ 1,000
    + return값은 소문자입니다.
    + letter의 모스부호는 공백으로 나누어져 있습니다.
    + letter에 공백은 연속으로 두 개 이상 존재하지 않습니다.
    + 해독할 수 없는 편지는 주어지지 않습니다.
    + 편지의 시작과 끝에는 공백이 없습니다.

+ ✏️코드

```cpp
    #include <string>
    #include <vector>

    using namespace std;

    ///////////// My Solution
    string solution(string letter) {
        string answer = "";
        string morse[26] = { ".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",
                            ".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",
                            ".--","-..-","-.--","--.." };
        // a~z 97~122

        // 공백으로 나누어 담기
        vector<string> morseLetter;
        string tmp ="";
        for (char c : letter) {
            if (isspace(c)) {
                morseLetter.push_back(tmp);
                tmp = "";
            }
            else {
                tmp += c;
            }
        }
        morseLetter.push_back(tmp);

        // 하나씩 해석하기
        for (int i = 0; i < morseLetter.size(); i++) {
            for (int j = 0; j < 26; j++) {
                if (morseLetter[i] == morse[j]) {
                    answer += (j + 97);
                    
                }
            }
        }

        return answer;
    }

    ///////////// Professor Solution
    class NotFoundError{};

    char translate(string sub){
        vector<string> dic = { ".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",
                            ".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",
                            ".--","-..-","-.--","--.." };

        for(int i = 0; i < dic.size(); i++){
            if(sub == dic[i]) 
                return 'a' + i;
        }
        throw NotFoundError();      // 위에서 안걸리면 error를 던짐
        
    }

    string solution(string letter) {
        string answer = "";

        int i, j;
        for(i = 0; i < letter.length(); i = j + 1){
            for(j = i; j < letter.length(); j++){
                if(letter[j] == ' '){
                    answer += translate(letter.substr(i, j-i));
                    break;
                }
            }
            if(j == letter.length()){
                answer += translate(letter.substr(i, j-i+1));
            }
        }    

        return answer;
    }
```

<br/>

### 16. 순서쌍 개수 구하기

+ 문제 설명 

순서쌍이란 두 개의 숫자를 순서를 정하여 짝지어 나타낸 쌍으로 (a, b)로 표기합니다.  
자연수 n이 매개변수로 주어질 때 두 숫자의 곱이 n인 자연수 순서쌍의 개수를 return하도록 solution 함수를 완성해주세요.

+ ⚠️제한사항
    + 1 ≤ n ≤ 1,000,000


+ ✏️코드

```cpp

    ///////// My Solution & Professor Solution
    #include <string>
    #include <vector>

    using namespace std;

    int solution(int n) {
        int answer = 0;
        
        for (int i = 1; i <= n; i++) {
            if (n % i == 0) {
                answer++;
            }
        }


        return answer;
    }
```