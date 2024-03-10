### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/64064
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%B6%88%EB%9F%89-%EC%82%AC%EC%9A%A9%EC%9E%90

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<string> check_id[8];
//중첩을 없애기 위해 set사용
set<vector<string>> answer_id;

void dfs(int index, int idNum, vector<string> ban_id, vector<string> used_id)
{
    if(index == idNum)
    {
        //중첩 확인을 위해 sort진행
        sort(ban_id.begin(), ban_id.end());
        answer_id.insert(ban_id);
        return;
    }
    
    for(int i = 0; i < check_id[index].size(); i++)
    {
        bool isUsed = false;
        for(string temp : used_id)
        {
            if(check_id[index][i] == temp)
            {
                isUsed = true;
            }
        }
        if(!isUsed)
        {
            ban_id.push_back(check_id[index][i]);
            used_id.push_back(check_id[index][i]);
            dfs(index+1, idNum, ban_id, used_id);
            ban_id.erase(ban_id.end());
            used_id.erase(used_id.end());            
        }
    }
}

int solution(vector<string> user_id, vector<string> banned_id) {
    int answer = 0;
    
    //banned_id에 부합하는 아이디들 검색
    for(int i = 0; i < banned_id.size(); i++)
    {
        string curBanId = banned_id[i];
        for(string curId : user_id)
        {
            if(curId.size() == curBanId.size())
            {
                bool isBanned = true;
                for(int j = 0; j < curId.size(); j++)
                {
                    if(curBanId[j] != '*')
                    {
                        if(curId[j] != curBanId[j])
                        {
                            isBanned = false;
                        }
                    }
                }
                if(isBanned)
                {
                    check_id[i].push_back(curId);
                }
            }
        }
    }
    //부합하는 아이디들을 가지고 중첩되지 않는 아이디 목록을 만듬
    dfs(0, banned_id.size(), vector<string>(), vector<string>());
    //중첩되지 않는 아이디 목록의 수
    answer = answer_id.size();
    
    return answer;
}
```