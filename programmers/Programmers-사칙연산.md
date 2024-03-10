### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/1843
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%82%AC%EC%B9%99%EC%97%B0%EC%82%B0

```cpp
#include <bits/stdc++.h>
#include <vector>
#include <string>
using namespace std;

int solution(vector<string> arr)
{
    int answer = -1;
    int max_dp[102][102] = {0,};
    int min_dp[102][102] = {0,};
    int numSize = arr.size()/2+1;
    
    //배열 초기화 및 자기 자신만 계산하는 경우 입력
    for(int i = 0; i < numSize; i++)
    {
        for(int j = 0; j < numSize; j++)
        {
            if(i == j)
            {
                max_dp[i][j] = stoi(arr[i*2]);
                min_dp[i][j] = stoi(arr[i*2]);
            }
            else
            {
                max_dp[i][j] = -10000000;
                min_dp[i][j] = 10000000;
            }
        }
    }
    
    //dp최대값과 최소값을 입력해나가며 계산
    for(int index = 1; index < numSize; index++)
    {
        for(int i = 0; i < numSize - index; i++)
        {
            int j = index + i;
            for(int k = i; k < j; k++)
            {
                if(arr[k * 2 + 1] == "-")
                {
                    max_dp[i][j] = max(max_dp[i][j], max_dp[i][k] - min_dp[k+1][j]);
                    min_dp[i][j] = min(min_dp[i][j], min_dp[i][k] - max_dp[k+1][j]);
                }
                else if(arr[k * 2 + 1] == "+")
                {
                    max_dp[i][j] = max(max_dp[i][j], max_dp[i][k] + max_dp[k+1][j]);
                    min_dp[i][j] = min(min_dp[i][j], min_dp[i][k] + min_dp[k+1][j]);
                }
            }
        }
    }
    
    answer = max_dp[0][numSize-1];
    return answer;
}
```