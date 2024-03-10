### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/72416
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%A7%A4%EC%B6%9C-%ED%95%98%EB%9D%BD-%EC%B5%9C%EC%86%8C%ED%99%94

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> team[300001];
int dp[300001][2] = {0,};
vector<int> salesInfo;

void dfs(int cur)
{
    dp[cur][0] = 0;
    dp[cur][1] = salesInfo[cur-1];
    if(team[cur].empty())   { return; }
    int ans = 0, minValue = 2100000000;
    
    for(int i = 0; i < team[cur].size(); i++)
    {
        dfs(team[cur][i]);
        if(dp[team[cur][i]][0] < dp[team[cur][i]][1])
        {
            dp[cur][0] += dp[team[cur][i]][0];
            dp[cur][1] += dp[team[cur][i]][0];
            minValue = min(minValue, dp[team[cur][i]][1] - dp[team[cur][i]][0]);
        }
        else
        {
            dp[cur][0] += dp[team[cur][i]][1];
            dp[cur][1] += dp[team[cur][i]][1];
            minValue = 0;
        }
    }
    dp[cur][0] += minValue;
}

int solution(vector<int> sales, vector<vector<int>> links) {
    int answer = 0;
    salesInfo = sales;
    
    for(vector<int> v : links)
    {
        team[v[0]].push_back(v[1]);
    }
    
    memset(dp, -1, sizeof(dp));
    
    dfs(1);
    
    return answer = min(dp[1][0], dp[1][1]);
}
```