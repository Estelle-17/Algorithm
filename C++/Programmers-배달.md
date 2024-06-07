### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12978
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%B0%B0%EB%8B%AC

```cpp
#include <bits/stdc++.h>
#include <iostream>
#include <vector>
using namespace std;

int solution(int N, vector<vector<int> > road, int K) {
    int answer = 0;
    int roadMap[51][51] = {0,};
    priority_queue<pair<int, int>> pq;
    int dist[51];
    
    //배열 초기화
    for(int i = 1; i <= N; i++)
    {
        for(int j = 1; j <= N; j++)
        {
            if(i == j)
            {
                roadMap[i][j] = 0;
            }
            else
            {
                roadMap[i][j] = 100000000;
            }
        }
        dist[i] = 100000000;
    }
    
    //배열에 간선등록
    for(vector<int> v : road)
    {
        if(roadMap[v[0]][v[1]] > v[2])
        {
            roadMap[v[0]][v[1]] = v[2];
        }
        if(roadMap[v[1]][v[0]] > v[2])
        {
            roadMap[v[1]][v[0]] = v[2];
        }
    }
    
    dist[1] = 0;
    pq.push(make_pair(0, 1));
    
    //다익스트라 알고리즘 실행
    while(!pq.empty())
    {
        int cost = pq.top().first;
        int currentVillage = pq.top().second;
        pq.pop();
        for(int i = 1; i <= N; i++)
        {
            int nextCost = roadMap[currentVillage][i];
            if(dist[i] > cost + nextCost)
            {
                dist[i] = cost + nextCost;
                pq.push(make_pair(cost + nextCost, i));
            }
        }
    }
    
    //K값보다 작은 값들 수 체크
    for(int i = 1; i <= N; i++)
    {
         if(dist[i] <= K)
         {
             answer++;
         }
    }

    return answer;
}
```