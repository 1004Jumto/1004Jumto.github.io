---
title: "[C++ Programming] 실습Part2: 방문 길이"
excerpt: "[C++ Programming] 14주차 2차시"
categories: [Cpplecture]
tags: [cpp, study]
toc: true
toc_sticky: true
---

## 방문 길이

+ 📝문제 설명  

    + 게임 캐릭터를 4가지 명령어를 통해 움직이려 합니다. 명령어는 다음과 같습니다.
        > U: 위쪽으로 한 칸 가기  
        > D: 아래쪽으로 한 칸 가기  
        > R: 오른쪽으로 한 칸 가기  
        > L: 왼쪽으로 한 칸 가기  

    + 캐릭터는 좌표평면의 (0, 0) 위치에서 시작합니다. 좌표평면의 경계는 왼쪽 위(-5, 5), 왼쪽 아래(-5, -5), 오른쪽 위(5, 5), 오른쪽 아래(5, -5)로 이루어져 있습니다.
	+ 이때, 우리는 게임 캐릭터가 지나간 길 중 캐릭터가 처음 걸어본 길의 길이를 구하려고 합니다.  
    예를 들어 위의 예시에서 게임 캐릭터가 움직인 길이는 9이지만, 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다. (8, 9번 명령어에서 움직인 길은 2, 3번 명령어에서 이미 거쳐 간 길입니다)  
    단, 좌표평면의 경계를 넘어가는 명령어는 무시합니다.

<br/>

+ ⚠️제한사항
    + dirs는 string형으로 주어지며, 'U', 'D', 'R', 'L' 이외에 문자는 주어지지 않습니다.
    + dirs의 길이는 500 이하의 자연수입니다.

<br/>

+ 📜입출력 예


   |  dirs        |       answer      | 
   | :-----------: | :---------------: | 
   | ULURRDLLU  |   7 |
   | LULLLLLLU  |   7 |

<br/>

+ ✏️풀이

```cpp
    #include <string>
    #include <vector>
    #include <iostream>

    using namespace std;

    class OutOfBound {};

    class Path
    {
    public:
        Path(int x1, int y1, int x2, int y2);
        friend bool operator==(Path& a, Path& b);

    private:
        int x1, y1, x2, y2;
    };

    Path::Path(int x1, int y1, int x2, int y2) :x1(x1), y1(y1), x2(x2), y2(y2) { ; }

    bool operator==(Path& a, Path& b)
    {
        if (a.x1 == b.x1 && a.y1 == b.y1 && a.x2 == b.x2 && a.y2 == b.y2) {
            return true;
        }

        if (a.x1 == b.x2 && a.y1 == b.y2 && a.x2 == b.x1 && a.y2 == b.y1) {
            return true;
        }

        return false;
    }

    /* Path를 계속 집어넣는 맞춤형 자료구조와 같음 */
    class PathQue
    {
    public:
        void push(Path& a);		// 기존에 동일한 path가 저장된 것이 있는지 검사해야함
        int count();

    private:
        vector<Path> paths;
        bool isIn(Path& a);
    };

    bool PathQue::isIn(Path& a) {
        for (int i = 0; i < paths.size(); i++) {
            if (paths[i] == a) {
                return true;
            }
        }
        return false;
    }

    void PathQue::push(Path& a) {
        if (!isIn(a)) {
            paths.push_back(a);
        }
    }

    int PathQue::count() {
        return paths.size();
    }

    /* 문제를 직접적으로 풀기 위한 클래스 */
    class Game
    {
    public:
        Game();
        void move(string& commands);
        int getX();
        int getY();
        int getCount();

    private:
        int x, y;
        void move(char c);
        PathQue que;
    };

    Game::Game() : x(0), y(0) { ; }

    void Game::move(string& commands)
    {
        for (int i = 0; i < commands.length(); i++) {
            try
            {
                int preX = x;
                int preY = y;
                move(commands[i]);
                Path temp(preX, preY, x, y);
                que.push(temp);
            }
            catch (OutOfBound& e) {

            }
        }
    }

    int Game::getX() {	return x;  }

    int Game::getY() {  return y;  }

    int Game::getCount()
    {
        return que.count();
    }

    void Game::move(char c)
    {
        switch (c) {
        case 'U':
            if (y == 5) throw OutOfBound();
            else {
                y++;
            }
            break;

        case 'D':
            if (y == -5) throw OutOfBound();
            else {
                y--;
            }
            break;

        case 'R':
            if (x == 5) throw OutOfBound();
            else {
                x++;
            }
            break;

        case 'L':
            if (x == -5) throw OutOfBound();
            else {
                x--;
            }
            break;
        }
    }

    int main(void) {
        Game game;
        string commands = "LULLLLLLU";

        game.move(commands);
        
        cout << "(" << game.getX() << ", " << game.getY() << ") : " << game.getCount() << endl;

        /* Path 만든거 테스트 */
        /*
        Path path1(1, 2, 3, 4), path2(3, 4, 1, 2);
        cout << (path1 == path2) << endl;
        
        Path path3(1, 2, 3, 4), path4(1, 2, 3, 4);
        cout << (path3 == path4) << endl;
        
        Path path5(1, 2, 3, 4), path6(3, 4, 1, 1);
        cout << (path5 == path6) << endl;
        */

        Path p1(1, 2, 3, 4), p2(1, 2, 5, 6), p3(3, 4, 1, 2);
        PathQue myque;
        myque.push(p1);
        myque.push(p2);
        myque.push(p3);
        cout << myque.count() << endl;

        return 0;
    }
```

## C++의 3대 중요 특성

+ 캡슐화, 모듈화
    각각의 기능적으로 구분되는 부분은 클래스로 나누어져 구성된다.
    각각의 클래스는 남의 클래스를 알지못하는 캡슐화가 지원된다.
    모듈화 -> 유지보수에 좋다.
    이 인풋에 대해서는 특정 아웃풋만 리턴하면 된다.

+ 다형성
    개념적으로 같은 기능을 갖는다면 같은 이름을 사용하는 것을 허용한다.

+ 상속
    기존의 코드에서 바뀌면 안되는 부분은 그대로 재사용할 수 있도록 한다.
    따라서 생산성이 향상된다.
    중복 코딩을 막아주고, 코드의 재사용을 체계화 시켜준다.
    일부 코드의 변동이 생긴 경우 복사 붙여넣기 할 필요가 없어진다.