### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/43238
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%9E%85%EA%B5%AD%EC%8B%AC%EC%82%AC

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

long long solution(int n, vector<int> times) {
    long long answer = 0;
    
    long long high = 1000000000000000000;   //1e18
    long long low = 0;
    long long mid;
    
    long long sum;
    while(high >= low)
    {
        mid = (high+low) / 2;
        
        sum = 0;
        for(int time : times)
        {
            sum += mid / time;
        }
        if(sum >= n)
        {
            high = mid - 1;
        }else
        {
            low = mid + 1;
        }
    }
    
    answer = high+1;
    return answer;
}
```