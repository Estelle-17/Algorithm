### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42897
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%8F%84%EB%91%91%EC%A7%88

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> money) {
    int answer = 0;
    vector<int> dp1(1000001, 0);    //첫번째 원소를 포함하고 마지막 원소를 제외한 값
    vector<int> dp2(1000001, 0);    //첫번째 원소를 제외하고 마지막 원소를 포함한 값
    
    dp1[0] = 0;
    dp1[1] = money[0];
    dp1[2] = max(money[1], money[0]);
    dp2[0] = 0;
    dp2[1] = money[1];
    dp2[2] = max(money[1], money[2]);
    //index의 집의 최대값 = 전집의 최대값과 (전전집의 최대값+현재 집의 값)중 최대값
    for(int i = 3; i < money.size(); i++)
    {
        dp1[i] = max(dp1[i-1], dp1[i-2] + money[i-1]);
        dp2[i] = max(dp2[i-1], dp2[i-2] + money[i]);
    }
    
    answer = max(dp1[money.size()-1], dp2[money.size()-1]);
    return answer;
}
```