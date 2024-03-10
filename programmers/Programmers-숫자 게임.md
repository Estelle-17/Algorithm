### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12987
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%88%AB%EC%9E%90-%EA%B2%8C%EC%9E%84

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> A, vector<int> B) {
    int answer = 0;
    int idx = 0;
    
    sort(A.begin(), A.end());
    sort(B.begin(), B.end());
    
    for(int i = 0; i < B.size() && idx < A.size(); i++)
    {
        if(A[idx] < B[i])   idx++;
    }
    answer = idx;
    
    return answer;
}
```