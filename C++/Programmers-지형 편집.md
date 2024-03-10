### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12984
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%A7%80%ED%98%95-%ED%8E%B8%EC%A7%91

```cpp
#include <bits/stdc++.h>
#include<vector>
using namespace std;

long long solution(vector<vector<int> > land, int P, int Q) {
    long long answer = LLONG_MAX;
    vector<long long> landNum;
    long long sum = 0;
    int n = land.size();
    
    for(int i = 0; i < n; i++)
    {
        for(int j = 0; j < n; j++)
        {
            landNum.push_back(land[i][j]);
            sum += land[i][j];
            //rightSum.second++;
            //rightSum.first += land[i][j] * Q;
        }
    }
    sort(landNum.begin(), landNum.end());

    long long lastLand = -1;    //이전에 계산한 층수
    long long blocks = 0;    //이전에 계산한 블록들의 수
    for(int i = 0; i < landNum.size(); i++)
    {
        if(lastLand != landNum[i])
        {
            //(landNum[i] * i) : 쌓을 높이를 맞출 시 존재햐아하는 블록의 수
            long long left = (landNum[i] * i) - blocks;
            //(landNum[i] * (landNum.size() - i)) -> 블록을 빼서 높이를 맞출 시 존재햐아하는 블록의 수
            //sum - blocks -> 빼는 블록들의 수
            long long right = sum - blocks - (landNum[i] * (landNum.size() - i));
            long long value  = left * P + right * Q;
            if(answer > value)
                answer = value;
            //다음 높이 값 저장
            lastLand = landNum[i];
        }
        //왼쪽 블록들의 수 저장
        blocks += landNum[i];
    }
    
    return answer;
}
```