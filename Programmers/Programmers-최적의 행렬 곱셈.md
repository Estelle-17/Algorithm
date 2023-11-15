### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12942
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%B5%9C%EC%A0%81%EC%9D%98-%ED%96%89%EB%A0%AC-%EA%B3%B1%EC%85%88

```cpp
#include<bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int answer;
vector<vector<int>> matrix;
int dp[201][201];

int solve(int n)
{
    int a, b;
    for(int i = 0; i < n; i++)
    {
        for(int j = 0; j < n-i; j++)
        {
            a = j;
            b = j+i;
            if(a == b)
            {
                dp[a][b] = 0;
            }
            else
            {
                dp[a][b] = 2100000000;
                for(int k = a; k < b; k++)
                {
                    dp[a][b] = min(dp[a][b], dp[a][k] + dp[k+1][b] + (matrix[a][0] * matrix[k][1] * matrix[b][1]));
                }
            }
        }
    }
    return dp[0][n-1];
}

int solution(vector<vector<int>> matrix_sizes) {
    int answer = 0;
    matrix = matrix_sizes;
    
    answer = solve(matrix_sizes.size());
    
    return answer;
}
```