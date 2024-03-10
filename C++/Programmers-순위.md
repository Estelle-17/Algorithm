### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/49191
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%88%9C%EC%9C%84

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(int n, vector<vector<int>> results) {
    int answer = 0;
    int resultMap[101][101] = {0,};
    
    for(int i = 0; i <= n; i++)
    {
        for(int j = 0; j <= n; j++)
        {
            resultMap[i][j] = 0;
        }
    }
    //맵에 승패를 입력해줌
    for(vector<int> v : results)
    {
        resultMap[v[0]][v[1]] = 1;
        resultMap[v[1]][v[0]] = -1;
    }
    
    for(int i = 1; i <= n; i++)
    {
        for(int j = 1; j <= n; j++)
        {
            if(i != j)
            {
                //만약 경기 결과가 있을 경우
                if(resultMap[j][i] != 0)
                {
                    int curResult = resultMap[j][i];
                    for(int k = 1; k <= n; k++)
                    {
                        if(resultMap[i][k] == curResult)
                        {
                            resultMap[j][k] = curResult;
                        }
                    }
                }
            }
        }
    }
    //경기 결과가 나오지 않은 선수들을 찾아 빼줍니다.
    answer = n;
    for(int i = 1; i <= n; i++)
    {
        for(int j = 1; j <= n; j++)
        {
            if(resultMap[i][j] == 0 && i != j)
            {
                answer--;
                break;
            }
        }
    }
    
    return answer;
}
```