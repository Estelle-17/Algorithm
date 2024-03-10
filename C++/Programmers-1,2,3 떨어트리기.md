### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/150364
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-123-%EB%96%A8%EC%96%B4%ED%8A%B8%EB%A6%AC%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int arriveNum[101] = {0,};
queue<int> node[101];

vector<int> solution(vector<vector<int>> edges, vector<int> target) {
    vector<int> answer;
    int targetcnt = 0;
    //다음 간선을 찾기 위해 오름차순으로 정렬
    sort(edges.begin(), edges.end());
    
    for(vector<int> v : edges)
    {
        node[v[0]].push(v[1]);
    }
    for(int n : target)
    {
        if(n != 0)  targetcnt++;
    }
    
    vector<int> nodeSequence;
    
    while(true)
    {
        int cur = 1;
        while(!node[cur].empty())
        {
            int t = node[cur].front();
            node[cur].push(t);
            node[cur].pop();
            cur = t;
        }
        arriveNum[cur-1]++;
        nodeSequence.push_back(cur-1);
        int cnt = 0;
        //여태까지 떨어트린 숫자로 target을 만들어 낼 수 있는지 확인
        for(int i = 0; i < target.size(); i++)
        {
            if(arriveNum[i] != 0)
            {
                if(arriveNum[i] > target[i])
                {
                    answer.push_back(-1);
                }
                else if(arriveNum[i] <= target[i] && arriveNum[i]*3 >= target[i])
                {
                    cnt++;
                }
            }
            if(!answer.empty())
            {
                break;
            }
        }
        if(cnt == targetcnt || !answer.empty())
        {
            break;
        }
    }
    if(answer.empty())
    {
        //1,2,3 카드를 사전 순으로 넣기 위해 dp표를 제작
        //j번 카드를 사용하여 i값을 구할 때 제일 처음으로 넣어야 하는 카드를 입력
        int dp[102][102] = {0,};
        for(int i = 1; i < 102; i++)
        {
            for(int j = 1; j < 102; j++)
            {
                dp[i][j] = 1;
                
                if(i == j * 3 - 1)  dp[i][j] = 2;
                else if(i == j * 3)  dp[i][j] = 3;
                if(i < j)   break;
            }
        }
        //순서대로 카드를 넣어줌
        for(int i = 0; i < nodeSequence.size(); i++)
        {
            int cur = nodeSequence[i];
            answer.push_back(dp[target[cur]][arriveNum[cur]]);
            target[cur] -= dp[target[cur]][arriveNum[cur]];
            arriveNum[cur]--;
        }
    }
    
    return answer;
}
```