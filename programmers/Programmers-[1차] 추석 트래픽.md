### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/17676
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-1%EC%B0%A8-%EC%B6%94%EC%84%9D-%ED%8A%B8%EB%9E%98%ED%94%BD

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

struct Time
{
    int hour;
    int minute;
    float second;
    float processingTime;
};

bool compareTime(Time a, Time b)
{
    float aTime = (a.hour * 3600) + (a.minute * 60) + a.second + 1 - 0.001f;
    float bTime = (b.hour * 3600) + (b.minute * 60) + b.second - b.processingTime + 0.001f;
    return aTime >= bTime;
}

int solution(vector<string> lines) {
    int answer = 0;
    vector<Time> timelines;
    
    sort(lines.begin(), lines.end());
    for(string str : lines)
    {
        Time t;
        t.hour = stoi(str.substr(11, 2));
        t.minute = stoi(str.substr(14, 2));
        t.second = stof(str.substr(17, 6));
        t.processingTime = stof(str.substr(24, 5));
        timelines.push_back(t);
    }
    int cnt = 0;
    
    for(int i = 0; i < timelines.size(); i++)
    {
        Time curTime = timelines[i];
        for(int j = i; j < timelines.size(); j++)
        {
            if(i != j && compareTime(curTime, timelines[j]))
            {
                cnt++;
            }
        }
        answer = max(answer, cnt+1);
        cnt = 0;
    }
    
    return answer;
}
```