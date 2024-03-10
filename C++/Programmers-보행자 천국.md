### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/1832
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%B3%B4%ED%96%89%EC%9E%90-%EC%B2%9C%EA%B5%AD

```cpp
#include <bits/stdc++.h>
#include <vector>

using namespace std;

int MOD = 20170805;

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
int solution(int m, int n, vector<vector<int>> city_map) {
    int answer = 0;
    //first : 오른쪽으로 오던 자동차, second : 아래로 오던 자동차
    vector<vector<pair<int, int>>> dp(500, vector<pair<int, int>>(500, make_pair(0, 0)));
    
    dp[0][0] = make_pair(1, 1);
    //0번째 줄에 값 넣어줌
    for(int i = 1; i < n; i++)
    {
        if(city_map[0][i] != 1)
        {
            dp[0][i].first = dp[0][i-1].first;
        }else
        {
            break;
        }
    }
    
    for(int i = 1; i < m; i++)
    {
        if(city_map[i][0] != 1)
        {
            dp[i][0].second = dp[i-1][0].second;
        }else
        {
            break;
        }
    }
    
    for(int i = 1; i < m; i++)
    {
        for(int j = 1; j < n; j++)
        {
            //벽일 경우 이동 가능한 수는 없음
            if(city_map[i][j] == 1)
            {
                dp[i][j] = make_pair(0, 0);
                continue;
            }
            
            //전에 움직였던 방향이랑 같은 방향으로 움직일 경우에는 1만 아니면 무조건 이동 가능
            //0이라면 모두 더해줌
            dp[i][j].second = (dp[i-1][j].second) % MOD;
            if(city_map[i-1][j] == 0)
            {
                dp[i][j].second += (dp[i-1][j].first) % MOD;
            }
            
  
            dp[i][j].first = (dp[i][j-1].first) % MOD;
            if(city_map[i][j-1] == 0)
            {
                dp[i][j].first += (dp[i][j-1].second) % MOD;
            }
            
        }
    }
    
    answer = (dp[m-1][n-1].first + dp[m-1][n-1].second) % MOD;
    
    return answer;
}
```