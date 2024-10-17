### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/250136
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%84%9D%EC%9C%A0-%EC%8B%9C%EC%B6%94

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<vector<int>> landMap;
vector<int> checkGetOil;
int oilCount;

void CheckOilCount(int x, int y)
{
    int dirX[4] = {0, 0, 1, -1};
    int dirY[4] = {1, -1, 0, 0};
    
    //시추관을 꽂으면 석유를 뽑을 수 있는 위치 저장
    checkGetOil[y] = 1;
    
    oilCount++;
    
    landMap[x][y] = 0;
    for(int i = 0; i < 4; i++)
    {
        if(x + dirX[i] < 0 || x + dirX[i] >= landMap.size() || y + dirY[i] < 0 || y + dirY[i] >= landMap[0].size())
            continue;
        
        if(landMap[x + dirX[i]][y + dirY[i]] == 1)
        {
            CheckOilCount(x + dirX[i], y + dirY[i]);
        }
    }
}

int solution(vector<vector<int>> land) {
    int answer = 0;
    vector<int> totalHeightOil(land[0].size(), 0);
    
    landMap = land;
    oilCount = 0;
    
    checkGetOil.assign(land[0].size(), 0);
    
    for(int i = 0; i < landMap.size(); i++)
    {
        for(int j = 0; j < landMap[i].size(); j++)
        {
            if(landMap[i][j] == 1)
            {
                oilCount = 0;
                CheckOilCount(i, j);
                for(int k = 0; k < checkGetOil.size(); k++)
                {
                    if(checkGetOil[k] == 1)
                    {
                        totalHeightOil[k] += oilCount;
                    }
                }
                checkGetOil.assign(land[0].size(), 0);
            }
        }
    }
    
    for(int n : totalHeightOil)
    {
        answer = answer < n ? n : answer;
    }
    
    return answer;
}
```