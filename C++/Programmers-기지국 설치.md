### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12979
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B8%B0%EC%A7%80%EA%B5%AD-%EC%84%A4%EC%B9%98

```cpp
#include <bits/stdc++.h>
#include <iostream>
#include <vector>
using namespace std;

int solution(int n, vector<int> stations, int w){
    int answer = 0;
    int spreadSize = w * 2 + 1;
    
    //시작 지점
    int start = 1;
    for(int i = 0; i < stations.size(); i++)
    {
        //설치된 기지국의 맨 뒤의 전파 범위
        int end = stations[i] - w - 1;
        if(start <= end)    //설치된 기지국의 맨 뒤의 전파 범위보다 시작 지점이 뒤에 있을 경우 기지국 추가 설치
        {
            int len = end - start + 1;
            if(len <= spreadSize)
                answer += 1;
            else
            {
                answer += len / spreadSize;
                if(len % spreadSize != 0)
                {
                    answer += 1;
                }
            }
        }
        //설치된 기지국의 맨 앞의 전파 범위
        start = stations[i] + w + 1;
    }
    //마지막 기지국의 전파 범위가 마지막 아파트까지 전파가 닿았는지 확인
    if(start <= n)
    {
        int len = n - start + 1;
            if(len <= spreadSize)
                answer += 1;
            else
            {
                answer += len / spreadSize;
                if(len % spreadSize != 0)
                {
                    answer += 1;
                }
            }
    }
    
    return answer;
}
```