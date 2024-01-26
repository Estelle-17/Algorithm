### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/150369
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%83%9D%EB%B0%B0-%EB%B0%B0%EB%8B%AC%EA%B3%BC-%EC%88%98%EA%B1%B0%ED%95%98%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

long long solution(int cap, int n, vector<int> deliveries, vector<int> pickups) {
    long long answer = 0;
    int deliverySum = 0;
    int pickupSum = 0;
    
    for(int i = n-1; i >= 0; i--)
    {
        int cnt = 0;
        deliverySum += deliveries[i];
        pickupSum += pickups[i];
        
        while(deliverySum > 0 || pickupSum > 0)
        {
            deliverySum -= cap;
            pickupSum -= cap;
            cnt++;
        }
        answer += (i+1) * 2 * cnt;
    }
    
    return answer;
}
```