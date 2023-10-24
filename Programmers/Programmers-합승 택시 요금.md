### 출저 - https://school.programmers.co.kr/learn/courses/30/lessons/72413
## 사용언어 - C++

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(int n, int s, int a, int b, vector<vector<int>> fares) {
    int answer = 2100000000;
    int shortistPoint[201][201] = {0,};
    
    //배열 초기화
    for(int i = 1; i <= n; i++)
    {
        for(int j = 1; j <= n; j++)
        {
            if(i == j)
            {
                shortistPoint[i][j] = 0;
            }
            else
            {
                shortistPoint[i][j] = 100000000;
            }
        }
    }
    //배열에 간선들 등록
    for(vector<int> v : fares)
    {
        shortistPoint[v[0]][v[1]] = v[2];
        shortistPoint[v[1]][v[0]] = v[2];
    }
    
    //k = 거치는 간선, i, j는 시작과 도착 지점
    for(int k = 1; k <= n; k++)
    {
        for(int i = 1; i <= n; i++)
        {
            for(int j = 1; j <= n; j++)
            {
                if(shortistPoint[i][j] > shortistPoint[i][k] + shortistPoint[k][j])
                {
                    shortistPoint[i][j] = shortistPoint[i][k] + shortistPoint[k][j];
                }
            }
        }
    }
    //합승해서 가는 지점과 도착한 지점으로부터 a, b 까지 요금의 최소값을 구함
    answer = shortistPoint[s][a] + shortistPoint[s][b];
    for(int i = 1; i <= n; i++)
    {
        int t = shortistPoint[s][i] + shortistPoint[i][a] + shortistPoint[i][b];
        if(answer > t)
        {
            answer = t;
        }
    }
    
    return answer;
}
```