### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42898
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%93%B1%EA%B5%A3%EA%B8%B8
```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(int m, int n, vector<vector<int>> puddles) {
    int answer = 0;
    vector<vector<int>> map(n+2, vector<int>(m+2, 0));
    vector<vector<int>> dp(n+2, vector<int>(m+2, 0));
    int INF = 1000000007;
    
    for(vector<int> v : puddles)
    {
        map[v[1]][v[0]] = 1;
    }
    
    dp[1][1] = 1;
    for(int i = 1; i <= n; i++)
    {
        for(int j = 1; j <= m; j++)
        {
            if(map[i][j] == 1)
                continue;
            dp[i][j] = (dp[i][j] + dp[i-1][j] % INF + dp[i][j-1] % INF) % INF;
        }
    }
    
    return dp[n][m] % INF;
}
```