### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/258705
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%82%B0-%EB%AA%A8%EC%96%91-%ED%83%80%EC%9D%BC%EB%A7%81

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(int n, vector<int> tops) {
    int answer = 0;
    vector<int> leftValue(n+1, 0);
    vector<int> rightValue(n+1, 0);
    
    //처음 삼각형의 갯수
    rightValue[1] = 1;
    leftValue[1] = tops[0] == 1 ? 3 : 2;
    //다음 삼각형의 갯수
    for(int i = 2; i <= tops.size(); i++)
    {
        if(tops[i-1] == 1)
            leftValue[i] = ((3 * leftValue[i-1]) + (2 * rightValue[i-1])) % 10007;
        else
            leftValue[i] = ((2 * leftValue[i-1]) + rightValue[i-1]) % 10007;
        rightValue[i] = (leftValue[i-1] + rightValue[i-1]) % 10007;
    }
    answer = (leftValue[n] + rightValue[n]) % 10007;
    return answer;
}
```