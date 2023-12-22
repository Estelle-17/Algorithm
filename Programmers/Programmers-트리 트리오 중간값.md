### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/68937
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%8A%B8%EB%A6%AC-%ED%8A%B8%EB%A6%AC%EC%98%A4-%EC%A4%91%EA%B0%84%EA%B0%92

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> tree[250001];
queue<int> lastNode;

int BFS(int n)
{
    int visited[250001] = {0,};
    queue<int> q;
    q.push(n);
    visited[n] = 1;
    int cnt = -1;   //노드의 거리 계산
    while(!q.empty())
    {
        cnt++;
        int qSize = q.size();
        lastNode = q;
        for(int i = 0; i < qSize; i++)
        {
            int curVal = q.front();
            q.pop();
            visited[curVal] = 1;
            for(int val : tree[curVal])
            {
                if(visited[val] == 0)
                {
                    q.push(val);
                    visited[val] = 1;
                }
            }
        }
    }
    return cnt;
}

int solution(int n, vector<vector<int>> edges) {
    int leafNode = 1;
    
    for(vector<int> v : edges)
    {
        tree[v[0]].push_back(v[1]);
        tree[v[1]].push_back(v[0]);
    }
    
    //탐색을 위해 제일 먼 리프 노드 검색
    BFS(1);
    
    //제일 먼 리프 노드를 가지고 제일 먼 노드 다시 검색
    int dist = BFS(lastNode.front());
    //이때 먼 노드가 2개 이상이면 게산한 거리를 return
    if(lastNode.size() > 1)
        return dist;
    
    //제일 먼 리프 노드를 가지고 제일 먼 노드 다시 검색
    dist = BFS(lastNode.front());
    //이때 먼 노드가 2개 이상이면 게산한 거리를 return
    //먼 노드가 1개라면 바로 전 노드가 중간값이 됨
    if(lastNode.size() > 1)
        return dist;
    
    return dist-1;
}
```