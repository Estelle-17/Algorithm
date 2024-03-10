### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/43164
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%97%AC%ED%96%89%EA%B2%BD%EB%A1%9C

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

unordered_map<string, queue<string>> airportTickets;
vector<string> answer;
int cnt;

void solve(string curAirport, vector<string> result, unordered_map<string, queue<string>> Tickets)
{
    //현재 있는 공항을 넣어줌
    result.push_back(curAirport);
    //만약 현재 있는 공항에서 다른 곳으로 이동이 가능할 경우
    auto iter = Tickets.find(curAirport);
    if(iter != Tickets.end())
    {
        if(!iter->second.empty())
        {
            //갈 수 있는 곳을 한곳씩 가보며 길을 찾음
            int qSize = iter->second.size();
            for(int i = 0; i < qSize; i++)
            {
                string str = iter->second.front();
                iter->second.pop();
                solve(str, result, Tickets);
                iter->second.push(str);
            }
        }
    }
    //만약 모든 항공권을 사용했을 경우
    if(result.size() == cnt)
    {
        //비어있으면 넣어줌
        if(answer.empty())
        {
            answer = result;
        }else
        {
            //하나씩 비교하여 알파벳 순으로 앞선 공항이 있을 경우 바꿔줌
            for(int i = 0; i < answer.size(); i++)
            {
                if(answer[i] > result[i])
                {
                    answer = result;
                    break;
                }else if(answer[i] < result[i])
                {
                    break;
                }
            }
        }
    }
}

vector<string> solution(vector<vector<string>> tickets) {
    
    cnt = tickets.size()+1;
    
    //항공권들을 노드에 연결하듯 배열에 저장해줌
    for(vector<string> v : tickets)
    {
        auto iter = airportTickets.find(v[0]);
        if(iter != airportTickets.end())
        {
            iter->second.push(v[1]);
        }else
        {
            airportTickets.emplace(make_pair(v[0], queue<string>()));
            airportTickets[v[0]].push(v[1]);
        }
    }
    solve("ICN", vector<string>(), airportTickets);
    
    return answer;
}
```