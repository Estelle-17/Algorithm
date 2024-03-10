### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/17678
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-1%EC%B0%A8-%EC%85%94%ED%8B%80%EB%B2%84%EC%8A%A4

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

string timeSetting(int time)
{
    if(time < 10)
    {
        return to_string(0) + to_string(time);
    }
    else
    {
        return to_string(time);
    }
}

string solution(int n, int t, int m, vector<string> timetable) {
    string answer = "";
    priority_queue<string, vector<string>, greater<string>> pq;
    //시간이 빠른 순으로 정렬
    for(string str : timetable)
    {
        pq.push(str);
    }
    
    int hour = 0;
    int minute = 0;
    int curHour, curMinute;
    
    hour = 9;
    for(int i = 0; i < n; i++)
    {
        //셔틀버스가 오는 시간 계산
        if(i != 0)
        {
            minute += t;
            hour += minute / 60;
            minute = minute % 60;
        }
        for(int j = 0; j < m; j++)
        {
            //대기줄의 사람이 있는지 확인
            if(!pq.empty())
            {
                curHour = stoi(pq.top().substr(0, 2));
                curMinute = stoi(pq.top().substr(3, 2));
                //만약 버스가 오는 시간보다 더 빠른 시간으로 온 사람이 있는지 확인
                if(hour > curHour || hour >= curHour && minute >= curMinute)
                {
                    pq.pop();
                }
            }
            else
            {
                curHour = -1;
                curMinute = -1;
            }
        }
    }
    //대기줄에 사람이 없고 제일 늦은 버스에 빈 자리가 있다면
    if(curHour == -1 && curMinute == -1)
    {
        answer = timeSetting(hour) + ":" + timeSetting(minute);
    }
    else
    {
        if(curMinute == 0)
        {
            curMinute = 59;
            curHour--;
        }
        else
        {
            curMinute--;
        }
        //올 사람이 있지만 버스보다 늦을 경우
        if(hour < curHour || hour <= curHour && minute <= curMinute)
        {
            answer = timeSetting(hour) + ":" + timeSetting(minute);
        }
        else
        {
            answer = timeSetting(curHour) + ":" + timeSetting(curMinute);
        }
    }
    
    return answer;
}
```