### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/87694
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%95%84%EC%9D%B4%ED%85%9C-%EC%A4%8D%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int rectangleMap[110][110] = {0,};
int dirX[4] = {0, 0, 1, -1};
int dirY[4] = {1, -1, 0, 0};
int answer = 10000000;

void solve(int x, int y, int count)
{
    if(rectangleMap[x][y] == 4)
    {
        answer = min(answer, count);
        return;
    }
    
    for(int i = 0; i < 4; i++)
    {
        if(x + dirX[i] < 0 || x + dirX[i] >= 110 || y + dirY[i] < 0 || y + dirY[i] >= 110)  continue;
        
        if(rectangleMap[x+dirX[i]][y+dirY[i]] == 1 || rectangleMap[x+dirX[i]][y+dirY[i]] == 4)
        {
            rectangleMap[x][y] = 3;
            solve(x+dirX[i], y+dirY[i], count+1);
        }
    }
}

int solution(vector<vector<int>> rectangle, int characterX, int characterY, int itemX, int itemY) {
    int dx[8] = {1, 1, -1, -1, 0, 0, 1, -1};
    int dy[8] = {-1, 1, 1, -1, 1, -1, 0, 0};
    
    //맵에 테두리를 둘러싼 네모 그려주기
    for(int k = 0; k < rectangle.size(); k++)
    {
        int x1 = rectangle[k][0]*2;
        int x2 = rectangle[k][1]*2;
        int y1 = rectangle[k][2]*2;
        int y2 = rectangle[k][3]*2;
        for(int i = x1; i <= y1; i++)
        {
            for(int j = x2; j <= y2; j++)
            {
                rectangleMap[i][j] = 2;
            }
        }
    }
    //도형이 겹쳐 꺾이는 부분도 테두리로 표시해주기
    for(int i = 0; i < 110; i++)
    {
        for(int j = 0; j < 110; j++)
        {
            for(int k = 0; k < 8; k++)
            {
                if(i + dirX[k] < 0 || i + dirX[k] >= 110 || j + dirY[k] < 0 || j + dirY[k] >= 110)  continue;
                
                if(rectangleMap[i][j] == 2 && rectangleMap[i+dx[k]][j+dy[k]] == 0)
                {
                    rectangleMap[i][j] = 1;
                    break;
                }
            }
        }
    }
    rectangleMap[itemX*2][itemY*2] = 4;
    //테두리를 따라 도착지점까지 찾아줌
    solve(characterX*2, characterY*2, 0);
    
    return answer/2;
}
```