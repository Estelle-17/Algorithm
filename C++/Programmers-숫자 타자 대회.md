### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/136797
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%88%AB%EC%9E%90-%ED%83%80%EC%9E%90-%EB%8C%80%ED%9A%8C

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int INF = INT_MAX;
string number;
queue<vector<int>> q;
vector<vector<vector<int>>> dp;
vector<vector<int>> moveCost;
int dx[8] = {0, 0, 1, -1, 1, 1, -1, -1};
int dy[8] = {1, -1, 0, 0, 1, -1, 1, -1};
int numpad[4][3] = {{1, 2, 3},
                    {4, 5, 6},
                    {7, 8, 9},
                    {-1, 0, -1}};

vector<int> calcMoveCost(int x, int y)
{
    vector<vector<int>> movingCost(4, vector<int>(3, 1000));
    movingCost[x][y] = 0;
    for(int l = 0; l < 2; l++)
    {
        for(int i = 0; i < 3; i++)
        {
            for(int j = 0; j < 3; j++)
            {
                for(int k = 0; k < 8; k++)
                {
                    if(i + dx[k] < 0 || i + dx[k] > 3 || j + dy[k] < 0 || j + dy[k] > 2)
                        continue;

                    if(numpad[i+dx[k]][j+dy[k]] >= 0 && numpad[i+dx[k]][j+dy[k]] <= 9)
                    {
                        if(k < 4)
                        {
                            movingCost[i+dx[k]][j+dy[k]] = min(movingCost[i+dx[k]][j+dy[k]], movingCost[i][j] + 2);
                        }else{
                            movingCost[i+dx[k]][j+dy[k]] = min(movingCost[i+dx[k]][j+dy[k]], movingCost[i][j] + 3);
                        }
                    }
                }
            }
        }
    }
    vector<int> v;
    v.push_back(movingCost[3][1]);
    for(int i = 0; i < 3; i++)
    {
        for(int j = 0; j < 3; j++)
        {
            v.push_back(movingCost[i][j]);
        }
    }
    return v;
}

void bfs(int index, int left, int right, int cost)
{
    queue<vector<int>> q;
    q.push({left, right});
    for(int i = 0; i < number.size(); i++)
    {
        int qSize = q.size();
        for(int j = 0; j < qSize; j++)
        {
            vector<int> cur = q.front();    q.pop();
            int curNum = number[i]-48;  //다음으로 눌러야 되는 수
            int prevCost = 0;   //전에 계산했던 오른손과 왼손 위치의 값
            if(i != 0)
                prevCost = dp[i-1][cur[0]][cur[1]];
            int leftCost = moveCost[cur[0]][curNum];    //왼손을 눌러야 되는 수로 움직였을 때의 가중치
            int rightCost = moveCost[cur[1]][curNum];   //오른손을 눌러야 되는 수로 움직였을 때의 가중치
            if(cur[1] != curNum)
            {
                if(prevCost + leftCost < dp[i][curNum][cur[1]])     //전에 계산했던 값보다 작을 경우
                {
                    if(dp[i][curNum][cur[1]] == INF)    //중첩되는 왼손 오른손이 있을 경우에는 값만 변경
                        q.push({curNum, cur[1]});
                    
                    dp[i][curNum][cur[1]] = prevCost + leftCost;
                }
            }

            if(cur[0] != curNum)
            {
                if(prevCost + rightCost < dp[i][cur[0]][curNum])     //전에 계산했던 값보다 작을 경우
                {
                    if(dp[i][cur[0]][curNum] == INF)    //중첩되는 왼손 오른손이 있을 경우에는 값만 변경
                        q.push({cur[0], curNum});
                    
                    dp[i][cur[0]][curNum] = prevCost + rightCost;
                }
            }
        }
    }
}

int solution(string numbers) {
    int answer = INF;
    int index = 1;
    number = numbers;
    
    //자판에서 자판까지 이동할 경우 생기는 가중치 계산
    moveCost.push_back({1, 7, 6, 7, 5, 4, 5, 3, 2, 3});
    for(int i = 0; i < 3; i++)
    {
        for(int j = 0; j < 3; j++)
        {
            moveCost.push_back(calcMoveCost(i, j));
            moveCost[index][index] = 1;
            index++;
        }
    }
    
    int left = 4;
    int right = 6;
    dp.assign(numbers.size(), vector<vector<int>>(10, vector<int>(10, INF)));
    
    bfs(0, left, right, 0);
    
    //최솟값을 찾아 return
    for(int i = 0; i < 10; i++)
    {
        for(int j = 0; j < 10; j++)
        {
            answer = min(answer, dp[number.size()-1][i][j]);
        }
    }
    
    return answer;
}
```