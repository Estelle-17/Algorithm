### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/152995
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%9D%B8%EC%82%AC%EA%B3%A0%EA%B3%BC

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

bool compare(vector<int> a, vector<int> b){
    if(a[0] > b[0])
        return true;
    else if(a[0] == b[0])
        return a[1] < b[1];
    else
        return false;
}

int solution(vector<vector<int>> scores) {
    int answer = 1;
    
    //등수를 찾는 사원의 근무태도와 평가점수 저장
    int workScore = scores[0][0];
    int peerScore = scores[0][1];
    
    //근무태도를 내림차순, 평가점수를 오름차순으로 정렬해줌
    sort(scores.begin(), scores.end(), compare);
    
    int maxVal = -1;
    for(int i = 0; i < scores.size(); i++)
    {
        if(maxVal < scores[i][1])   //평가점수 제일 큰 값을 저장해줌
        {
            maxVal = scores[i][1];
        }else if(maxVal > scores[i][1]) //근무태도는 내림차순으로 정렬했으니 무조건 같거나 낮으며, 평가점수도 낮다면 인센티브에서 제외
        {
            if(workScore == scores[i][0] && peerScore == scores[i][1])  //만약 등수를 찾는 사원이 인센티브에서 제외될 시
                return -1;
            continue;
        }

        if(workScore+peerScore < scores[i][0]+scores[i][1]) //두 점수의 합이 더 높은 사원 검색
            answer++;
    }
    
    return answer;
}
```