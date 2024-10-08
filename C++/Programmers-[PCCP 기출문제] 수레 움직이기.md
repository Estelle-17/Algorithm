### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/250134
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EC%88%98%EB%A0%88-%EC%9B%80%EC%A7%81%EC%9D%B4%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

#define INF 100000000

vector<vector<int>> RedMaze, BlueMaze;
deque<pair<int, int>> RedRoute, BlueRoute;
pair<int, int> RedStart, RedEnd, BlueStart, BlueEnd;

int MinRouteCount = INF;
int RedCount;

void CheckEnableRoute()
{
    deque<pair<int, int>> rRoute = RedRoute;
    deque<pair<int, int>> bRoute = BlueRoute;
    pair<int, int> red, blue, prevRed, prevBlue;
    
    int count = rRoute.size() > bRoute.size() ? rRoute.size() : bRoute.size();
    
    red = RedStart;
    blue = BlueStart;
        
    while(!rRoute.empty() || !bRoute.empty())
    {
        if(!rRoute.empty())
        {
            prevRed = red;
            red = rRoute.front();
            rRoute.pop_front();
        }
        
        if(!bRoute.empty())
        {
            prevBlue = blue;
            blue = bRoute.front();
            bRoute.pop_front();
        }
        
        //지나갈 수 없는 경우 체크
        if(red.first == blue.first && red.second == blue.second ||
           prevRed.first == blue.first && prevRed.second == blue.second && red.first == prevBlue.first && red.second == prevBlue.second)
        {
            return;
        }
    }
    
    if(MinRouteCount > count)
    {
        MinRouteCount = count;
    }
}

void FindBlueRoute(int x, int y, int count)
{
    if(x == BlueEnd.first && y == BlueEnd.second)
    {
        BlueRoute.push_back({x, y});
        CheckEnableRoute();
        BlueRoute.pop_back();
        
        return;
    }
        
    int dirX[4] = {1, -1, 0, 0};
    int dirY[4] = {0, 0, 1, -1};
    
    BlueMaze[x][y] = count;
    
    for(int i = 0; i < 4; i++)
    {
        if(x + dirX[i] < 0 || x + dirX[i] >= BlueMaze.size() || y + dirY[i]< 0 || y + dirY[i] >= BlueMaze[0].size())
            continue;
        
        //두 개의 수레가 겹치는 경우
        if(BlueMaze[x + dirX[i]][y + dirY[i]] == 0)
        {
            BlueRoute.push_back({x + dirX[i], y + dirY[i]});
            FindBlueRoute(x + dirX[i], y + dirY[i], count+1);
            BlueRoute.pop_back();
        }
    }
    
    BlueMaze[x][y] = 0;
}

void FindRedRoute(int x, int y, int count)
{
    if(x == RedEnd.first && y == RedEnd.second)
    {
        RedRoute.push_back({x, y});
        FindBlueRoute(BlueStart.first, BlueStart.second, 1);
        RedRoute.pop_back();
        
        return;
    }
        
    int dirX[4] = {1, -1, 0, 0};
    int dirY[4] = {0, 0, 1, -1};
    
    RedMaze[x][y] = count;
    
    for(int i = 0; i < 4; i++)
    {
        if(x + dirX[i] < 0 || x + dirX[i] >= RedMaze.size() || y + dirY[i] < 0 || y + dirY[i] >= RedMaze[0].size())
            continue;
        
        if(RedMaze[x + dirX[i]][y + dirY[i]] == 0)
        {
            RedRoute.push_back({x + dirX[i], y + dirY[i]});
            FindRedRoute(x + dirX[i], y + dirY[i], count+1);
            RedRoute.pop_back();
        }
    }
    
    RedMaze[x][y] = 0;
}

int solution(vector<vector<int>> maze) {
    int answer = 0;
    RedCount = 0;
    
    RedMaze.assign(maze.size(), vector<int>(maze[0].size(), 0));
    BlueMaze.assign(maze.size(), vector<int>(maze[0].size(), 0));
    
    //시작 지점, 도착 지점, 벽 탐색
    for(int i = 0; i < maze.size(); i++)
    {
        for(int j = 0; j < maze[0].size(); j++)
        {
            if(maze[i][j] == 1)
            {
                RedStart = {i, j};
            }else if(maze[i][j] == 3)
            {
                RedEnd = {i, j};
            }else if(maze[i][j] == 2)
            {
                BlueStart = {i, j};
            }else if(maze[i][j] == 4)
            {
                BlueEnd = {i, j};
            }else if(maze[i][j] == 5)
            {
                RedMaze[i][j] = -1;
                BlueMaze[i][j] = -1;
            }
        }
    }
    
    FindRedRoute(RedStart.first, RedStart.second, 1);
    
    if(MinRouteCount != INF)
    {
        answer = MinRouteCount-1;
    }
    
    return answer;
}
```