### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/118667
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%91%90-%ED%81%90-%ED%95%A9-%EA%B0%99%EA%B2%8C-%EB%A7%8C%EB%93%A4%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> queue1, vector<int> queue2) {
    int answer = -1;
    long long q1sum = 0;
    long long q2sum = 0;
    queue<int> q1;
    queue<int> q2;
    
    //각각의 queue에 값 저장 및 각 큐의 누적합을 더함
    for(int n : queue1){
        q1sum += n;
        q1.push(n);
    }
    for(int n : queue2){
        q2sum += n;
        q2.push(n);
    }
    //queue1.size()의 4배만큼 값을 검색
    for(int i = 0; i < (queue1.size() << 2); i++)
    {
        if(q1sum < q2sum){
            int curNum = q2.front(); q2.pop();
            q1sum += curNum;
            q2sum -= curNum;
            q1.push(curNum);
        }else if(q1sum > q2sum){
            int curNum = q1.front(); q1.pop();
            q2sum += curNum;
            q1sum -= curNum;
            q2.push(curNum);
        }else{
            return ++answer;
        }
        answer++;
    }
    
    return -1;
}
```