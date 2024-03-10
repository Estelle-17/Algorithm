### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/64062
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%A7%95%EA%B2%80%EB%8B%A4%EB%A6%AC-%EA%B1%B4%EB%84%88%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> stones, int k) {
    int answer = 200000001;
    int result = 0;
    //first : index, second : value
    deque<pair<int, int>> dq;
    int cnt = 0;
    
    for(int i = 0; i < stones.size(); i++)
    {
        //만약 디딤돌 중 제일 큰 숫자가 칸 수를 넘어갈 경우 제거
        while(!dq.empty() && dq[0].first < i-k+1)
        {
            dq.pop_front();
        }
        //제일 큰 숫자 다음으로 올 수 있는 최대값을 넣기 위해 작은 수들을 제거
        while(!dq.empty() && dq.back().second < stones[i])
        {
            dq.pop_back();
        }
        dq.push_back(make_pair(i, stones[i]));
        
        //넘어갈 수 없는 칸 수에 도달할 경우부터 최솟값을 찾아줌
        if(i >= k-1 && answer > dq[0].second)
        {
            answer = dq[0].second;
        }
    }
    
    return answer;
}
```