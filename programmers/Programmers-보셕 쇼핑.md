### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/67258
## 사용언어 - C++
## 해설 -https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%B3%B4%EC%84%9D-%EC%87%BC%ED%95%91

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(vector<string> gems) {
    vector<int> answer;
    set<string> s;
    unordered_map<string, int> gemIndex;
    
    //보석의 종류가 몇가지인지 검색
    for(string str : gems)
    {
        s.insert(str);
    }
    
    int gemSize = s.size();
    answer.push_back(1);
    answer.push_back(100001);
    queue<string> q;
    int start = 0;
    for(int i = 0; i < gems.size(); i++)
    {
        //이미 구매했던 보석일 경우 갯수를 더해줌
        auto iter = gemIndex.find(gems[i]);
        if(iter != gemIndex.end())
        {
            iter->second++;
        }else
        {
            //구매했던 보석이 아닐 경우 map에 추가
            gemIndex.emplace(make_pair(gems[i], 1));
        }
        //구매한 보석들을 순서대로 push해줌
        q.push(gems[i]);
        
        //만약 제일 앞의 보석을 2가지 이상 구매했을 경우 앞에 있는 보석을 하나씩 빼줌
        while(true)
        {
            if(gemIndex.find(q.front())->second > 1)
            {
                gemIndex.find(q.front())->second--;
                q.pop();
                //앞에서 하나씩 보석을 뺐으므로 시작지점도 이동해줌
                start++;
            }else
            {
                break;
            }
        }
        
        //만약 보석의 모든 종류를 구매했을 경우
        if(gemIndex.size() == gemSize)
        {
            if(answer[1]-answer[0] > i-start)
            {
                answer[0] = start;
                answer[1] = i;
            }
        }
    }
    answer[0]++;
    answer[1]++;
    return answer;
}
```