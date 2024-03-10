### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/132266
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%B6%80%EB%8C%80%EB%B3%B5%EA%B7%80

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n, vector<vector<int>> roads, vector<int> sources, int destination) {
    vector<int> answer;
    vector<int> area[n+1];
    queue<int> q;
    vector<int> visit(n+1, 0);
    map<int, int> sourcesMap;
    
    //간선을 배열에 등록
    for(vector<int> v : roads)
    {
        area[v[0]].push_back(v[1]);
        area[v[1]].push_back(v[0]);
    }
    //거리를 찾아야하는 값들 해쉬값으로 저장
    for(int n : sources)
    {
        sourcesMap.emplace(make_pair(n, -1));
    }
    
    int cnt = 0;
    q.push(destination);
    map<int, int>::iterator iter;
    while(!q.empty())
    {
        int qSize = q.size();
        for(int i = 0; i < qSize; i++)
        {
            int cur = q.front();
            q.pop();
            
            //찾아야하는 값을 찾으면 거리 등록
            iter = sourcesMap.find(cur);
            if(iter != sourcesMap.end())
            {
                if(iter->second > cnt || iter->second == -1)
                {
                    iter->second = cnt;
                }
            }
            
            //이미 지나간 길은 체크
            visit[cur] = 1;
            for(int n : area[cur])
            {
                if(visit[n] == 0)
                {
                    q.push(n);
                }
            }
        }
        cnt++;
    }
    //sources순서대로 값 등록
    for(int s : sources)
    {
        answer.push_back(sourcesMap[s]);
    }
    
    return answer;
}
```