### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12927
## 사용언어 - C++
## 해설- https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%95%BC%EA%B7%BC-%EC%A7%80%EC%88%98

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

long long solution(int n, vector<int> works) {
    long long answer = 0;
    priority_queue<int> pq;
    
    for(int i = 0; i < works.size(); i++)
    {
        pq.push(works[i]);
    }
    
    for(int i = 0; i < n; i++)
    {
        int time = pq.top();
        pq.pop();
        time--;
        pq.push(time);
    }
    
    for(int i = 0; i < works.size(); i++)
    {
        int temp = pq.top();
        pq.pop();
        if(temp > 0)
        {
            answer += temp * temp;
        }
    }
    
    return answer;
}
```