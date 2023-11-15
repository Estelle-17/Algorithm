### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42861
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%84%AC-%EC%97%B0%EA%B2%B0%ED%95%98%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

bool compare(vector<int> a, vector<int> b) {return a[2] < b[2];}

int solution(int n, vector<vector<int>> costs) {
    int answer = 0;
    int check[100];
    int cnt = 0;
    
    for(int i = 0; i < n; i++)
    {
        check[i] = i;
    }
    
    sort(costs.begin(), costs.end(), compare);
    
    for(int i = 0; i < costs.size(); i++)
    {           
        if(check[costs[i][0]] != check[costs[i][1]])
        {
            int bigTemp, smallTemp;
            answer += costs[i][2];
            if(check[costs[i][0]] < check[costs[i][1]])
            {
                bigTemp = check[costs[i][1]];
                smallTemp = check[costs[i][0]];
            }
            else
            {
                bigTemp = check[costs[i][0]];
                smallTemp = check[costs[i][1]];
                check[costs[i][0]] = check[costs[i][1]];
            }
            for(int j = 0; j < n; j++)
            {
                if(check[j] == bigTemp)
                {
                    check[j] = smallTemp;
                }
            }
            cnt++;
        }
        if(cnt == n - 1)
        {
            break;
        }
    }
    
    return answer;
}
```