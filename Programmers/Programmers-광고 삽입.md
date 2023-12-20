### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/72414
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B4%91%EA%B3%A0-%EC%82%BD%EC%9E%85

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

struct Time{
    long long startTime;
    long long endTime;
        
    Time(long long a, long long b) : startTime(a), endTime(b)  {}
};

bool compare(Time a, Time b)
{
    return a.startTime < b.startTime;
}

string secondToTime(long long second)
{
    string str;
    int t = second / 3600;
    if(t < 10)
        str = "0" + to_string(t) + ":";
    else
        str = to_string(t) + ":";
    t = (second % 3600) / 60;
    if(t < 10)
        str += "0" + to_string(t) + ":";
    else
        str += to_string(t) + ":";
    t = (second % 3600) % 60;
    if(t < 10)
        str += "0" + to_string(t);
    else
        str += to_string(t);
    
    return str;
}

vector<int> timeLog(360000, 0);

string solution(string play_time, string adv_time, vector<string> logs) {
    string answer = "";
    
    long long answerSecond = 0;
    long long adTime = 0;
    long long playTime = 0;
    
    //시간을 초로 바꾼 후 구간을 배열 내에 더해줌
    for(int i = 0; i < logs.size(); i++)
    {
        Time t = Time(stoi(logs[i].substr(0, 2)) * 3600 + stoi(logs[i].substr(3, 2)) * 60 + stoi(logs[i].substr(6, 2)),
                           stoi(logs[i].substr(9, 2)) * 3600 + stoi(logs[i].substr(12, 2)) * 60 + stoi(logs[i].substr(15, 2)));
        for(int j = t.startTime; j < t.endTime; j++)
        {
            timeLog[j]++;
        }
    }
    
    //총 동영상 시간과 광고 시간도 초로 바꿔줌
    adTime = stoi(adv_time.substr(0, 2)) * 3600 + stoi(adv_time.substr(3, 2)) * 60 + stoi(adv_time.substr(6, 2));
    playTime = stoi(play_time.substr(0, 2)) * 3600 + stoi(play_time.substr(3, 2)) * 60 + stoi(play_time.substr(6, 2));
    
    //같다면 맨 처음 시간이 정답
    if(adTime == playTime)
    {
        return "00:00:00";
    }
    
    long long maxSum = 0;
    long long timeSum = 0;
    //맨 처음이 시작시간일 경우의 누적 재생시간
    for(int i = 0; i < adTime; i++)
    {
        timeSum += (long long)timeLog[i];
    }
    maxSum = timeSum;
    
    for(int i = adTime; i <= playTime; i++)
    {
        //시작시간을 1초씩 더하며 누적 재생시간의 값을 비교
        timeSum += (long long)(timeLog[i] - timeLog[i-adTime]);
        if(maxSum < timeSum)
        {
            maxSum = timeSum;
            answerSecond = i-adTime+1;
        }
    }
    //초로 되있는 값을 시간으로 변환
    answer = secondToTime(answerSecond);   
    
    return answer;
}
```