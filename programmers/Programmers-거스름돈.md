### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12907
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B1%B0%EC%8A%A4%EB%A6%84%EB%8F%88

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(int n, vector<int> money) {
    int answer = 0;
    long long arr[100001] = {0,};
    
    sort(money.begin(), money.end());
    arr[0] = 1;
    for(int i = 0; i < money.size(); i++)
    {
        int cur = money[i];
        for(int j = cur; j <= n; j++)
        {
            arr[j] = arr[j] + arr[j - cur];
        }
    }
    
    answer = arr[n] % 1000000007;
    
    return answer;
}
```