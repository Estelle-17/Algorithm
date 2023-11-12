### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12938
## 사용언어 - C++

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n, int s) {
    vector<int> answer;
    int divValue, divRest;
    
    if(s < n)
    {
        answer.push_back(-1);
    }
    else
    {
        divValue = s / n;
        divRest = s % n;
        for(int i = 0; i < n; i++)
        {
            answer.push_back(divValue);
        }
        int size = answer.size();
        for(int i = 0; i < divRest; i++)
        {
            size--;
            answer[size]++;
        }
    }
    
    return answer;
}
```