### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12929
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%98%AC%EB%B0%94%EB%A5%B8-%EA%B4%84%ED%98%B8%EC%9D%98-%EA%B0%AF%EC%88%98

```cpp
#include <string>
#include <vector>

using namespace std;

int solution(int n) {
    int answer = 0;
    int bracket[15] = {0,};
    bracket[0] = 1;
    bracket[1] = 1;
    bracket[2] = 2;
    bracket[3] = 5;

    for(int i = 4; i <= n; i++)
    {
        for(int j = 0; j < i; j++)
        {
            bracket[i] += bracket[j] * bracket[i-j-1];
        }
    }
    answer = bracket[n];
    
    return answer;
}
```