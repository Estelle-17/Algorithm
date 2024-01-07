### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12920
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%84%A0%EC%9E%85-%EC%84%A0%EC%B6%9C-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(int n, vector<int> cores) {
    int answer = 0;
    long long time = 0;
    
    //처리해야하는 일의 갯수가 코어보다 적다면 갯수만큼 return;
    if(n - cores.size() <= 0)
    {
        return n;
    }
    
    long long high = 21000000000;
    long long low = 0;
    while(high >= low)  //n값보다 적게 작업을 진행하는 시간의 최대값을 구함
    {
        long long mid = (high+low)/2;
        time = cores.size();
        for(int i = 0; i < cores.size(); i++)
        {
            time += mid / cores[i];
            if(time >= n)
                break;
        }
        
        if(time >= n)
            high = mid - 1;
        else
            low = mid + 1;            
        
    }
    //시간의 최대값을 가졌을 때의 처리량을 구함
    time = cores.size();
    for(int i = 0; i < cores.size(); i++)
    {
        time += high / cores[i];
    }
    //이후 마지막 작업을 처리하는 코어 탐색
    for(int i = 0; i < cores.size(); i++)
    {
        if((high+1) % cores[i] == 0)
            time++;
        if(time == n)
            return i+1;
    }
    
    return answer;
}
```