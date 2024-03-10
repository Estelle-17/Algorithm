### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/118669
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%93%B1%EC%82%B0%EC%BD%94%EC%8A%A4-%EC%A0%95%ED%95%98%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n, vector<vector<int>> paths, vector<int> gates, vector<int> summits) {
    vector<int> answer;
    pair<int, int> result = make_pair(0, 10000001);
    vector<pair<int, int>> mountain[50001];
    int gate[50001] = {0,};
    int summit[50001] = {0,};
    
    //간선 등록
    for(vector<int> v : paths)
    {
        mountain[v[0]].push_back(make_pair(v[1], v[2]));
        mountain[v[1]].push_back(make_pair(v[0], v[2]));
    }
    //출입구와 산봉우리의 빠른 확인을 위해 배열 등록
    for(int val : gates)
    {
        gate[val] = 1;
    }
     for(int val : summits)
    {
        summit[val] = 1;
    }
    
    //출발점에서 봉우리까지의 최단 거리 탐색
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> q;
    for(int i = 0; i < gates.size(); i++)
    {
        int cur = gates[i];
        q.push(make_pair(0, cur));  //first : intensity, second : 노드 위치
        vector<int> dp(50001, 10000001);    //출발점부터 index까지의 최단 거리 저장
        int visited[50001] = {0,};
        dp[cur] = 0;
        while(!q.empty())
        {
            pair<int, int> curVal = q.top();
            q.pop();
            if(summit[curVal.second] == 1)  //산봉우리일 경우 넘어감
                continue;
            visited[curVal.second] = 1;
            for(pair<int, int> p : mountain[curVal.second])
            {
                if(gate[p.first] == 1)  //출입구일 경우 넘어감
                    continue;
                int maxVal = max(curVal.first, p.second);   //intensity 계산
                if(maxVal > result.second)  //만약 intensity가 전 출입구로부터 찾았던 intensity보다 클 경우 넘어감
                    continue;
                if(maxVal < dp[p.first])    //dp에 저장된 값보다 작을 경우 새로 저장 및 경로 탐색
                {
                    dp[p.first] = maxVal;
                    q.push(make_pair(maxVal, p.first));
                }
            }
        }
        //산봉우리들을 찾아서 intensity의 최소값을 검색 후 저장
        for(int j = 0; j < summits.size(); j++)
        {
            if(dp[summits[j]] < result.second || dp[summits[j]] == result.second && summits[j] < result.first)
            {
                result = make_pair(summits[j], dp[summits[j]]);
            }
        }
    }
    
    answer.push_back(result.first);
    answer.push_back(result.second);
    
    return answer;
}
```