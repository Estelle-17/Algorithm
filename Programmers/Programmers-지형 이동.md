### 출저 - https://school.programmers.co.kr/learn/courses/30/lessons/62050
## 사용언어 - C++

```cpp
#include <iostream>
#include <cstdlib>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

//지형 간 연결할 수 있는 사다리 비용과 두 지형의 값 저장
struct ladder{
    int value;
    int startLand;
    int endLand;
};

vector<vector<int>> map;
vector<vector<int>> checkMap;
vector<ladder> checkLadderGroup;
//dfs의 4방향 값
const int dx[4] = {1, -1, 0, 0};
const int dy[4] = {0, 0, 1, -1};
int mapSize;
int parent[90000];

//sort를 위한 compare함수
bool compare(ladder a, ladder b) { return a.value < b.value; }

//union find를 위한 함수들
int findParent(int u)
{
    if(u == parent[u])  return u;
    return findParent(parent[u]);
}

void unionParent(int a, int b)
{
    a = findParent(a);
    b = findParent(b);
    a < b ? parent[b] = a : parent[a] = b;
}
//union find를 통한 비교
bool compareParent(int a, int b)
{
    a = findParent(a);
    b = findParent(b);
    return a == b;
}
//지형 탐색을 위한 dfs
void checkLand(int x, int y, int height, int level)
{
    checkMap[x][y] = level;
    for(int i = 0; i < 4; i++)
    {
        if(x+dx[i] >= 0 && y+dy[i] >= 0 && x+dx[i] < mapSize && y+dy[i] < mapSize)
        {
            int distance = abs(map[x][y] - map[x+dx[i]][y+dy[i]]);
            if(checkMap[x+dx[i]][y+dy[i]] == 0 && distance <= height)
            {
                checkLand(x+dx[i], y+dy[i], height, level);
            }
        }
    }
}
//4방향을 체크하여 다른 지형이 있을 경우 사다리를 넣을 수 있다고 판단하여 배열에 넣어줌
void checkLadder(int x, int y)
{
    for(int i = 0; i < 4; i++)
    {
        if(x+dx[i] >= 0 && y+dy[i] >= 0 && x+dx[i] < mapSize && y+dy[i] < mapSize &&
           checkMap[x][y] != -1 && checkMap[x+dx[i]][y+dy[i]] != -1)
        {
            if(checkMap[x][y] != checkMap[x+dx[i]][y+dy[i]])
            {
                int distance = abs(map[x][y] - map[x+dx[i]][y+dy[i]]);
                ladder l;
                l.value = distance;
                l.startLand = checkMap[x][y];
                l.endLand = checkMap[x+dx[i]][y+dy[i]];
                checkLadderGroup.push_back(l);
            }
        }
    }
    checkMap[x][y] = -1;
}

int solution(vector<vector<int>> land, int height) {
    int answer = 0;
    int landCount = 0;
    map = land;
    mapSize = land.size();
    
    //지형 계산을 위한 checkMap을 초기화시켜줌
    vector<int> t(mapSize, 0);
    for(int i = 0; i < mapSize; i++)
    {
        checkMap.push_back(t);
    }
    
    //dfs를 통해 사다리 없이 다닐 수 있는 지형을 checkMap에 기록함
    for(int i = 0; i < mapSize; i++)
    {
        for(int j = 0; j < mapSize; j++)
        {
            if(checkMap[i][j] == 0)
            {
                landCount++;
                checkLand(i, j, height, landCount);
            }
        }
    }
    //union find 계산를 위한 배열 초기화
    for(int i = 0; i < landCount; i++)
    {
        parent[i] = i;
    }
    
    //지형 간의 사다리를 놓는 경우의 수를 checkLadderGroup에 넣어줌
    for(int i = 0; i < mapSize; i++)
    {
        for(int j = 0; j < mapSize; j++)
        {
            if(checkMap[i][j] != 0)
            {
                checkLadder(i, j);
            }
        }
    }
    
    //비용을 기준으로 오름차순 정렬
    sort(checkLadderGroup.begin(), checkLadderGroup.end(), compare);
    
    int cnt = 0;
    //union find를 사용하여 연결된 지형들을 체크하며 값들을 순서대로 더해줌
    //지형 수-1만큼 더하면 사다리를 전부 찾았다는 뜻이므로 멈춤
    for(int i = 0; i < checkLadderGroup.size(); i++)
    {
        if(!compareParent(checkLadderGroup[i].startLand, checkLadderGroup[i].endLand))
        {
            answer += checkLadderGroup[i].value;
            unionParent(checkLadderGroup[i].startLand, checkLadderGroup[i].endLand);
            cnt++;
        }
        if(cnt == landCount-1)
        {
            break;
        }
    }

    return answer;
}
```