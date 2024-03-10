### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/131129
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%B9%B4%EC%9A%B4%ED%8A%B8-%EB%8B%A4%EC%9A%B4

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int target) {
    vector<int> answer;
    int dp[100001][2] = {0,};
    
    for(int i = 1; i <= target; i++)
    {
        if(i <= 20 || i == 50)
        {
            dp[i][0] = 1;
            dp[i][1] = 1;
        }else if(i <= 40 && i % 2 == 0)
        {
            dp[i][0] = 1;
            dp[i][1] = 0;
        }else if(i <= 60 && i % 3 == 0)
        {
            dp[i][0] = 1;
            dp[i][1] = 0;
        }else if(i >= 51 && i <= 70)
        {
            dp[i][0] = 2;
            dp[i][1] = 2;
        }else if(i < 70)
        {
            if(i < 40)
            {
                dp[i][0] = 2;
                dp[i][1] = 2;
            }else
            {
                dp[i][0] = 2;
                dp[i][1] = 1;
            }
        }else{
            //60을 쓰는 경우와 50을 쓰는 경우의 다트를 던진 수가 같을 경우
            if(dp[i-60][0] == dp[i-50][0])
            {
                dp[i][0] = dp[i-50][0]+1;
                dp[i][1] = max(dp[i-60][1], dp[i-50][1]+1);
            }else if(dp[i-60][0] < dp[i-50][0]) //60을 쓰는 경우가 50을 쓰는 경우보다 다트를 던진 수가 적을 경우
            {
                dp[i][0] = dp[i-60][0]+1;
                dp[i][1] = dp[i-60][1];
            }else   //60을 쓰는 경우가 50을 쓰는 경우보다 다트를 던진 수가 적을 경우
            {
                dp[i][0] = dp[i-50][0]+1;
                dp[i][1] = dp[i-50][1]+1;
            }
        }
    }
    
    answer.push_back(dp[target][0]);
    answer.push_back(dp[target][1]);
    
    return answer;
}
```