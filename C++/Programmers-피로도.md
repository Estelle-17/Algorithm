### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/87946?language=cpp#
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%94%BC%EB%A1%9C%EB%8F%84

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int answer;
int dungeonSize;

void solve(vector<vector<int>> &dungeons, vector<int> &checkMap, int remainStamina, int exploreCount){
    //탐험한 횟수가 던전 크기와 같을 경우 함수 종료
    if(exploreCount >= dungeonSize)
    {
        answer = exploreCount;
        return;
    }
    
    for(int i = 0; i < dungeonSize; i++)
    {
        //탐험할 수 있는 모든 던전 체크, 이미 들렸던 곳은 다시 가지 못함
        if(dungeons[i][0] <= remainStamina && checkMap[i] == 0)
        {
            checkMap[i] = 1;
            solve(dungeons, checkMap, remainStamina-dungeons[i][1], exploreCount+1);
            checkMap[i] = 0;
        }
        
        if(answer >= dungeonSize)
            return;
    }
    //탐험한 횟수가 이번 탐험 최대 횟수보다 클 경우 최대값 갱신
    if(answer < exploreCount)
    {
        answer = exploreCount;
    }
}

int solution(int k, vector<vector<int>> dungeons) {
    answer = -1;
    vector<int> vec(dungeons.size(), 0);
    dungeonSize = dungeons.size();
    
    for(int i = 0; i < dungeonSize; i++)
    {
        if(dungeons[i][0] <= k && vec[i] == 0)
        {
            vec[i] = 1;
            solve(dungeons, vec, k-dungeons[i][1], 1);
            vec[i] = 0;
        }
        
        if(answer >= dungeonSize)
            return answer;
    }
    
    return answer;
}
```