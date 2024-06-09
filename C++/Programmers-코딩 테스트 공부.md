### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/118668
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%BD%94%EB%94%A9-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EA%B3%B5%EB%B6%80

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(int alp, int cop, vector<vector<int>> problems) {
    int answer = 0;
    vector<vector<int>> dp(151, vector<int>(151, 2100000000));
    //도달해야하는 알고력과 코딩력
    int goal_alp = alp;
    int goal_cop = cop;
    
    for(int i = 0; i < problems.size(); i++)
    {
        goal_alp = max(goal_alp, problems[i][0]);
        goal_cop = max(goal_cop, problems[i][1]);
    }
    //아무 조건 없이 cost 1을 증가시켜 알고력이나 코딩력 둘 중 하나를 올릴 수 있음
    problems.push_back({0, 0, 1, 0, 1});
    problems.push_back({0, 0, 0, 1, 1});
    
    //시작점은 cost 0
    dp[alp][cop] = 0;
    
    for(int i = alp; i <= goal_alp; i++)
    {
        for(int j = cop; j <= goal_cop; j++)
        {
            //목표값에 도달하면 계산할 필요 없음
            if(i == goal_alp && j == goal_cop)
                continue;
            
            int curCost = dp[i][j];
            
            //문제들을 현재 값과 더하며 최소로 되는 값을 검색
            for(vector<int> v : problems)
            {
                if(i < v[0] || j < v[1])
                    continue;
                
                //더해서 맞춰야하는 최대값보다 넘어가지 않도록 비교
                int nextAlp = min(goal_alp, i + v[2]);
                int nextCop = min(goal_cop, j + v[3]);
                
                if(dp[nextAlp][nextCop] > curCost + v[4])
                    dp[nextAlp][nextCop] = curCost + v[4];
            }
        }
    }

    answer = dp[goal_alp][goal_cop];
    
    return answer;
}
```