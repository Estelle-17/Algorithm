### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/161988
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%97%B0%EC%86%8D-%ED%8E%84%EC%8A%A4-%EB%B6%80%EB%B6%84-%EC%88%98%EC%97%B4%EC%9D%98-%ED%95%A9
```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

long long solution(vector<int> sequence) {
    long long answer = 0;
    long long dp[2][500000];
    dp[0][0] = sequence[0];
    dp[1][0] = -sequence[0];
    
    answer = max(dp[0][0], dp[1][0]);
    
    for(int i = 1; i < sequence.size(); i++)
    {
        dp[0][i] = max((long long)sequence[i], dp[1][i-1] + sequence[i]);
        dp[1][i] = max((long long)-sequence[i], dp[0][i-1] - sequence[i]);
        answer = max(answer, dp[0][i]);
        answer = max(answer, dp[1][i]);
    }
    
    return answer;
}
```