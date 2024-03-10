### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/92345
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%82%AC%EB%9D%BC%EC%A7%80%EB%8A%94-%EB%B0%9C%ED%8C%90

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int boardSizeX;
int boardSizeY;
int dx[4] = {-1, 1, 0, 0};
int dy[4] = {0, 0, -1, 1};
vector<vector<int>> gameBoard;

int solve(int curX, int curY, int oppX, int oppY)
{
    //만약 보드 위치에 바닥이 없으면 리턴
    if(gameBoard[curX][curY] == 0)
    {
        return 0;
    }
    //반환해줄 값
    int ret = 0;
    for(int i = 0; i < 4; i++)
    {
        int nx = curX+dx[i];
        int ny = curY+dy[i];
        if(0 > nx || nx >= boardSizeX || 0 > ny || ny >= boardSizeY)    continue;
        
        if(gameBoard[nx][ny] == 1)
        {
            gameBoard[curX][curY] = 0;
            //정해진 방향으로 갔을 때의 값을 재귀로 구해줌
            int val = solve(oppX, oppY, nx, ny) + 1;
            gameBoard[curX][curY] = 1; 
            
            if(ret % 2 == 0 && val % 2 == 1)    //현재는 지고 있지만 구해온 값이 이길 수 있을 때
            {
                ret = val;
            }
            else if(ret % 2 == 0 && val % 2 == 0)   //현재는 지고 있고 구해온 값도 지고 있을 때
            {
                ret = max(ret, val);
            }
            else if(ret % 2 == 1 && val % 2 == 1)   //현재는 이기고 있고 구해온 값도 이기고 있을 때
            {
                ret = min(ret, val);
            }
        }
    }
    return ret;
}

int solution(vector<vector<int>> board, vector<int> aloc, vector<int> bloc) {
    int answer = -1;
    boardSizeX = board.size();
    boardSizeY = board[0].size();
    gameBoard = board;

    answer = solve(aloc[0], aloc[1], bloc[0], bloc[1]);
    
    return answer;
}
```