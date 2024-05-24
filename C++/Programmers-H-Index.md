### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42747
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-H-Index

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> citations) {
    int answer = 0;
    //오름차순으로 정렬
    sort(citations.begin(), citations.end());
    
    int index = citations.size()-1;
    int upCnt = 0;
    int downCnt = citations.size();
    
    //제일 큰 수부터 차례대로 내려가며 h값 탐색
    for(int i = citations[citations.size()-1]; i >= 0; i--)
    {
        if(citations[index] >= i)
        {
            upCnt++;
            downCnt--;
            index--;
        }
        //제일 높은 수부터 탐색했으므로 처음으로 찾은 값이 정답
        if(i <= upCnt && i >= downCnt)
        {
            answer = i;
            break;
        }
    }
    
    return answer;
}
```