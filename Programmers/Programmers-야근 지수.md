### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12927
## 사용언어 - C++

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