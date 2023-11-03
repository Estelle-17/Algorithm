### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/43162
## 사용언어 - C++

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(int n, vector<vector<int>> computers) {
    int answer = 0;
    int check[200];
    
    for(int i = 0; i < n; i++)
    {
        check[i] = i;
    }
    
    for(int i = 0; i < computers.size(); i++)
    {
        for(int j = 0; j < computers[i].size(); j++)
        {
            //컴퓨터와 연결된 부분을 찾아 배열로 표현
            if(computers[i][j] == 1)
            {
                int bigTemp, smallTemp;
                if(check[i] < check[j])
                {
                    bigTemp = check[j];
                    smallTemp = check[i];
                }
                else
                {
                    bigTemp = check[i];
                    smallTemp = check[j];
                }
                for(int k = 0; k < n; k++)
                {
                    if(check[k] == bigTemp)
                    {
                        check[k] = smallTemp;
                    }
                }
            }
        }
    }
    
    //set을 사용해 중복되는 수를 없애줌
    set<int> s;
    for(int i = 0; i < n; i++)
    {
        s.emplace(check[i]);
    }
    answer = s.size();
    
    return answer;
}
```