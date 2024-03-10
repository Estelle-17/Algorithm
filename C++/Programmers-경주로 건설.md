### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/67259
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B2%BD%EC%A3%BC%EB%A1%9C-%EA%B1%B4%EC%84%A4

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int INF = 2100000000;

int solution(vector<vector<int>> board) {
    int answer = INF;
    //[0] : x좌표, [1] : y좌표, [2] : 방향
    int direction[4][3] = {{0, 1, 0}, {1, 0, 1}, {0, -1, 2}, {-1, 0, 3}};
    int n = board.size();
    vector<vector<vector<int>>> dp(n, vector<vector<int>>(n, vector<int>(4, INF))); 
    queue<vector<int>> q;
    
    q.push({0, 0, 0, 0});
    q.push({0, 0, 0, 1});
    while(!q.empty())
    {
        vector<int> cur = q.front();    q.pop();
        int x = cur[0];
        int y = cur[1];
        int cost = cur[2];
        int dir = cur[3];
        for(int i = 0; i < 4; i++)
        {
            //계산해도 되는 부분인지 확인
            if(x+direction[i][0] < 0 || x+direction[i][0] >= n || y+direction[i][1] < 0 || y+direction[i][1] >= n)
                continue;
            
            //맵에 통행 가능한 부분만 검색
            if(board[x+direction[i][0]][y+direction[i][1]] == 0)
            {
                int newCost = cost+100;
                if(dir != direction[i][2])
                {
                    newCost += 500;
                }
                //현재 계산한 값보다 dp값이 더 클 경우
                if(dp[x+direction[i][0]][y+direction[i][1]][direction[i][2]] > newCost)
                {
                    dp[x+direction[i][0]][y+direction[i][1]][direction[i][2]] = newCost;
                    //도착지점일경우 queue에 저장X
                    if(x+direction[i][0] == n-1 && y+direction[i][1] == n-1)
                        continue;
                    q.push({x+direction[i][0], y+direction[i][1], newCost, direction[i][2]});
                }
            }
        }
    }
    //도착지점의 4방향 탐색 후 최솟값 반환
    for(int i = 0; i < 4; i++)
    {
        answer = min(answer, dp[n-1][n-1][i]);
    }
    
    return answer;
}
```