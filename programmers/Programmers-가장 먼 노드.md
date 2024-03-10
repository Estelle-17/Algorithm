### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/49189
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B0%80%EC%9E%A5-%EB%A8%BC-%EB%85%B8%EB%93%9C

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(int n, vector<vector<int>> edge) {
    int answer = 0;
    vector<int> tree[20001];
    vector<int> dist(20001, -1);
    queue<int> checkNode;
    
    for(vector<int> v : edge)
    {
        tree[v[0]].push_back(v[1]);
        tree[v[1]].push_back(v[0]);
    }
    
    dist[1] = 0;
    checkNode.push(1);
    while(!checkNode.empty())
    {
        int cur = checkNode.front();
        checkNode.pop();
        for(int i : tree[cur])
        {
            if(dist[i] == -1)
            {
                dist[i] = dist[cur] + 1;
                checkNode.push(i);
            }
        }
    }
    
    sort(dist.begin(), dist.end(), greater<>());
    int max = dist[0];
    
    for(int i : dist)
    {
        if(max == i)
        {
            answer++;
        }
    }
    
    return answer;
}
```