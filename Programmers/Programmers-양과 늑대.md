### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/92343
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%96%91%EA%B3%BC-%EB%8A%91%EB%8C%80

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> land[20];
vector<int> animalInfo;
int maxSheep = 0;

void solve(int sheep, int wolf, queue<int> q)
{
    //양의 최대값을 넣어줌
    if(maxSheep < sheep)
    {
        maxSheep = sheep;
    }
    
    int size = q.size();
    for(int i = 0; i < size; i++)
    {
        int cur = q.front();
        //양이 있을 경우
        if(animalInfo[cur] == 0)
        {
            q.pop();
            //양의 다음 노드들의 정보를 저장 후 재귀로 검색
            queue<int> temp = q;
            for(int num : land[cur])
            {
                temp.push(num);
            }    
            solve(sheep+1, wolf, temp);
            //다음 계산을 위해 다시 넣어줌
            q.push(cur);
        }
        else
        {
            //늑대가 양보다 같거나 많을 경우 계산 무시
            if(sheep <= wolf+1)
            {
                q.push(cur);
                q.pop();
            }
            else
            {
                q.pop();
                queue<int> temp = q;
                //늑대의 다음 노드들의 정보를 저장 후 재귀로 검색
                for(int num : land[cur])
                {
                    temp.push(num);
                }
                solve(sheep, wolf+1, temp);
                //다음 계산을 위해 다시 넣어줌
                q.push(cur);
            }
        }
    }
}

int solution(vector<int> info, vector<vector<int>> edges) {
    int answer = 0;
    animalInfo = info;

    //그래프 입력
    for(vector<int> v : edges)
    {
        land[v[0]].push_back(v[1]);
    }
    
    //0번째가 시작지점이고 무조건 양이 있으므로 처음만 미리 계산
    queue<int> q;
    for(int t : land[0])
    {
        q.push(t);
    }
    solve(1, 0, q);
    answer = maxSheep;
    
    return answer;
}
```