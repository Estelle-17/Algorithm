### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/67260
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%8F%99%EA%B5%B4-%ED%83%90%ED%97%98

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

bool solution(int n, vector<vector<int>> path, vector<vector<int>> order) {
    bool answer = true;
    vector<int> cave[n];
    vector<bool> visited(n, false);
    
    //간선 연결
    for(vector<int> v : path)
    {
        cave[v[0]].push_back(v[1]);
        cave[v[1]].push_back(v[0]);
    }
    
    //선 방문 동굴 설정
    vector<int> key(n, 0);
    vector<int> lock(n, 0);
    for(vector<int> v : order)
    {
        key[v[0]] = v[1];
        lock[v[1]] = v[0];
    }
    
    vector<int> visitedLock(n, 0);
    queue<int> q;
    if(lock[0] != 0)    //0번 방이 어느 동굴을 탐색해야 갈 수 있다면 탐험 불가능
        return false;
    
    q.push(0);
    while(!q.empty())
    {
        int qSize = q.size();
        for(int i = 0; i < qSize; i++)
        {
            int cur = q.front();    q.pop();
            visited[cur] = true;
            for(int num : cave[cur])
            {
                if(visited[num])
                    continue;
                
                if(lock[num] != 0)    //먼저 탐색해야 하는 동굴인 경우
                {
                    visitedLock[num] = 1;
                }else
                {
                    q.push(num);
                    if(key[num] != 0) //현재 방문한 동굴이 정해진 동굴일 경우
                    {
                        lock[key[num]] = 0;

                        if(visitedLock[key[num]] != 0)  //정해진 동굴을 방문하여 갈 수 있는 동굴에 탐색을 진행한 경우
                        {
                            q.push(key[num]);
                            visitedLock[key[num]] = 0;
                        }
                    }
                }
            }
        }
    }
    for(int i = 0; i < n; i++)
    {
        if(!visited[i])
            return false;
    }
    
    return answer;
}
```