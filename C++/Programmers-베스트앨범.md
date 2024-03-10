### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42579
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%B2%A0%EC%8A%A4%ED%8A%B8%EC%95%A8%EB%B2%94

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

struct genreMusic
{
    //first : 재생 횟수, second : 인덱스
    vector<pair<int, int>> playidx;
    int totalPlays;
};

bool compare(pair<int, int> a, pair<int, int> b)
{ 
    if(a.first > b.first || a.first == b.first && a.second < b.second)
    {
        return true;
    }
    return false;
}

bool genreCompare(pair<string, genreMusic> a, pair<string, genreMusic> b)   { return a.second.totalPlays > b.second.totalPlays; }

vector<int> solution(vector<string> genres, vector<int> plays) {
    vector<int> answer;
    unordered_map<string, genreMusic> genre;
    
    int idx = 0;
    for(string s : genres)
    {
        auto iter = genre.find(s);
        if(iter != genre.end())
        {
            iter->second.playidx.emplace_back(make_pair(plays[idx], idx));;
            iter->second.totalPlays += plays[idx];
        }else
        {
            genreMusic t;
            t.playidx.emplace_back(make_pair(plays[idx], idx));
            t.totalPlays = plays[idx];
            genre.emplace(make_pair(s, t));
        }
        idx++;
    }
    
    //정렬을 위해 벡터로 변환
    vector<pair<string, genreMusic>> vec;
    for(auto iter : genre)
    {
        vec.emplace_back(make_pair(iter.first, iter.second));
    }
    
    //plays의 합을 기준으로 내림차준
    sort(vec.begin(), vec.end(), genreCompare);
    
    for(pair<string, genreMusic> g : vec)
    {
        sort(g.second.playidx.begin(), g.second.playidx.end(), compare);
        for(int i = 0; i < g.second.playidx.size(); i++)
        {
            if(i == 2)  break;
            answer.emplace_back(g.second.playidx[i].second);
        }
    }
    
    return answer;
}
```