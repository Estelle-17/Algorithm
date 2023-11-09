### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12929
## 사용언어 - C++

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