### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/133500
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%93%B1%EB%8C%80

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> lighthouses[100001];
int answer; 

bool solve(int cur, int parent)
{
    int isUsingLight = false;
    
    //하위 노드들 검색
    for(int n : lighthouses[cur])
    {
        if(n != parent)
        {
            //만약 근처의 하위 노드들이 불을 키지 않았을 경우
            if(!solve(n, cur))
            {
                //현재 등불은 불이 켜져야 함
                isUsingLight = true;
            }
        }
    }
    
    //불이 켜졌을 때 갯수를 더해줌
    if(isUsingLight)
    {
        answer++;
    }
    
    return isUsingLight;
}

int solution(int n, vector<vector<int>> lighthouse) {
    answer = 0;
    
    //간선 등록
    for(vector<int> v : lighthouse)
    {
        lighthouses[v[0]].push_back(v[1]);
        lighthouses[v[1]].push_back(v[0]);
    }
    solve(1, 1);
    
    return answer;
}
```