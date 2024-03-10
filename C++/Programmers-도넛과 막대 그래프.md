### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/258711
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%8F%84%EB%84%9B%EA%B3%BC-%EB%A7%89%EB%8C%80-%EA%B7%B8%EB%9E%98%ED%94%84

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

unordered_map<int, vector<int>> node;
vector<int> inNodeCount(1000001, 0);

int dfs(int start)
{
    int nextNum = start;
    while(!node[nextNum].empty())
    {
        if(node[nextNum].size() >= 2)   //간선이 2개 이상일 경우 8자
            return 3;
        
        if(node[nextNum][0] == start)   //자기 자신에게 돌아올 경우 도넛
            return 1;
        else
            nextNum = node[nextNum][0]; //간선이 끝날 경우 막대
    }
    
    return 2;
}

vector<int> solution(vector<vector<int>> edges) {
    vector<int> answer(4, 0);
    //노드 입력
    unordered_map<int, vector<int>>::iterator iter;
    for(vector<int> v : edges)
    {
        iter = node.find(v[0]);
        if(iter == node.end())
            node.emplace(make_pair(v[0], vector<int>()));
        node[v[0]].push_back(v[1]);
        inNodeCount[v[1]]++;
    }
    //정점 탐색
    for(iter = node.begin(); iter != node.end(); iter++)
    {
        if(iter->second.size() >= 2 && inNodeCount[iter->first] == 0)
        {
            answer[0] = iter->first;
        }
    }
    
    for(int curNum : node[answer[0]])
    {
        answer[dfs(curNum)]++;
    }
    
    return answer;
}
```