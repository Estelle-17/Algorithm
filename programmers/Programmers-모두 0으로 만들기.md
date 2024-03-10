### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/76503
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%AA%A8%EB%91%90-0%EC%9C%BC%EB%A1%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<long long> indexNum;
vector<int> node[300000];
long long sum;

long long solve(int cur, int parent)
{
    
    long long curSum = indexNum[cur];
    for(int n : node[cur])
    {
        if(n != parent)
        {
            //자식 노드들의 가중치를 찾아 더해줌
            curSum += solve(n, cur);
        }
    }
    //현재까지 진행된 가중치를 전부 더해줌
    sum += abs(curSum);
    return curSum;
}

long long solution(vector<int> a, vector<vector<int>> edges) {
    long long answer = -2;
    
    //각 인덱스의 값들 저장
    for(int i = 0; i < a.size(); i++)
    {
        indexNum.push_back(a[i]);
    }
    
    sum = 0;
    for(int n : a)
    {
        sum += n;
    }
    //만약 모든 가중치를 더했을 때 0이 나오지 않으면 0 만드는 것이 불가능한 트리
    if(sum != 0)
    {
        answer = -1;
    }
    else
    {
        //각 간선들을 트리로 만들어줌
        for(vector<int> v : edges)
        {
            node[v[0]].push_back(v[1]);
            node[v[1]].push_back(v[0]);
        }
        
        solve(0, 0);
        
        answer = sum;
    }
    
    return answer;
}
```