### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42861
## 사용언어 - C++

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