### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/1837
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-GPS

```cpp
#include <bits/stdc++.h>
#include <vector>

using namespace std;

#define INF 100000

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
int solution(int n, int m, vector<vector<int>> edge_list, int k, vector<int> gps_log) {
    int answer = 0;
    //시간과 노드들의 수로 만들어진 DP배열
    vector<vector<int>> dp(k, vector<int>(n+1, INF));
    vector<int> road[n+1];
    
    //간선 등록
    for(vector<int> v : edge_list)
    {
        road[v[0]].push_back(v[1]);
        road[v[1]].push_back(v[0]);
    }
    //시작점은 언제나 0이고 택시가 머무를 수 있으므로 간선에 자기 자신 등록
    for(int i = 1; i <= n; i++)
    {
        road[i].push_back(i);
    }
    dp[0][gps_log[0]] = 0;
    for(int i = 1; i < k; i++)
    {
        int cur = gps_log[i];   //현재 거점
        for(int j = 1; j <= n; j++)
        {
            int prevValue = dp[i-1][j]; //전 거점의 값
            if(prevValue == INF)
                continue;

            for(int nextRoad : road[j]) //전 거점으로부터 움직일 수 있는 거점
            {
                if(cur == nextRoad) //만약 움직일 수 있는 거점이 현재 거점하고 같을 경우
                    dp[i][nextRoad] = min(dp[i][nextRoad], prevValue);  //경우의 수 최소값 등록
                else
                    dp[i][nextRoad] = min(dp[i][nextRoad], prevValue+1);    //거점에 가지 못할 경우 경로 변경을 했다고 가정하여 +1을 해준 후 최소값 등록
            }
        }
    }
    //gps의 마지막 값의 dp값을 return
    if(dp[k-1][gps_log[k-1]] == INF)
        answer = -1;
    else
        answer = dp[k-1][gps_log[k-1]];
    
    return answer;
}
```