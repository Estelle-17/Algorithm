### 출처 - https://www.acmicpc.net/problem/2839
## 사용언어 - C++
## 해설 - 

```C++17
#include <iostream>

using namespace std;

int main()
{
    int N;
    int answer = 0;
    cin >> N;

    while (N != 0)
    {
        if (N % 5 == 0)
        {
            answer += N / 5;
            break;
        }
        else if(N >= 3)
        {
            answer++;
            N = N - 3;
        }
        else
        {
            answer = -1;
            break;
        }
    }
    cout << answer;
}
```