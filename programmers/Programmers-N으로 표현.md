### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42895
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-N%EC%9C%BC%EB%A1%9C-%ED%91%9C%ED%98%84

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<long long> dp[9];
int Num;

void solve(int a, int b, int n)
{
    for(int i = 0; i < dp[a].size(); i++)
    {
        for(int j = 0; j < dp[b].size(); j++)
        {
            dp[n].push_back(dp[a][i] + dp[b][j]);
            dp[n].push_back(dp[a][i] - dp[b][j]);
            dp[n].push_back(dp[a][i] * dp[b][j]);
            if(dp[b][j] != 0)
            {
                dp[n].push_back(dp[a][i] / dp[b][j]);
            }
        }
    }
    dp[n].push_back((dp[n-1][dp[n-1].size()-1] * 10) + Num);
}

int solution(int N, int number) {
    int answer = 0;
    Num = N;
    dp[1].push_back(N);
    if(N == number)
    {
        return 1;
    }
    for(int k = 2; k <= 8; k++)
    {
        for(int i = 1; i < k; i++)
        {
            solve(i, k-i, k);  
        }
        for(int i = 0; i < dp[k].size(); i++)
        {
            if(dp[k][i] == number)
            {
                return k;
            }
        }
    }
    
    return -1;
}
```