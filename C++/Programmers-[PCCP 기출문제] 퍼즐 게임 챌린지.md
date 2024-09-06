### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/340212
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%8D%BC%EC%A6%90-%EA%B2%8C%EC%9E%84-%EC%B1%8C%EB%A6%B0%EC%A7%80

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> diffs, vector<int> times, long long limit) {
    int answer = 0;
    
    int high = 100000;
    int low = 1;
    int mid;
    long long int totalTime;
    
    while(high >= low)
    {
        mid = (high + low) / 2;
        totalTime = 0;
        for(int i = 0; i < diffs.size(); i++)
        {
            totalTime += times[i];
            if(mid < diffs[i])
            {
                totalTime += (times[i-1] + times[i]) * (diffs[i] - mid);
            }
            
            if(totalTime > limit)
            {
                break;
            }
        }
        
        if(totalTime > limit)
        {
            low = mid + 1;
        }
        else
        {
            high = mid - 1;
        }
    }
    
    answer = low;
    
    return answer;
}
```