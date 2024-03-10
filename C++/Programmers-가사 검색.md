### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/60060
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B0%80%EC%82%AC-%EA%B2%80%EC%83%89

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

class word
{
    public:
        int nextWordCount[26] = {0, };
        word* child[26] = {nullptr, };
        int count = 0;
};
class backword
{
    public:
        int prevWordCount[26] = {0, };
        backword* child[26] = {nullptr, };
        int count = 0;
};

word wordRoot[10001];
backword backwordRoot[10001];

void insertWord(string str)
{
    //접두사부터 시작해서 순서대로 단어 저장
    word* curNode = &wordRoot[str.size()];
    for(int i = 0; i < str.size(); i++)
    {
        if(curNode->child[str[i] - 'a'] != nullptr)
        {
            curNode->nextWordCount[str[i] - 'a']++;
            curNode->count++;
            curNode = curNode->child[str[i] - 'a'];
        }else
        {
            curNode->nextWordCount[str[i] - 'a']++;
            curNode->count++;
            curNode->child[str[i] - 'a'] = new word;
            curNode = curNode->child[str[i] - 'a'];
        }
    }
    //접미사부터 시작해서 순서대로 단어 저장
    backword* curBackNode = &backwordRoot[str.size()];
    for(int i = str.size()-1; i >= 0; i--)
    {
        if(curBackNode->child[str[i] - 'a'] != nullptr)
        {  
            curBackNode->prevWordCount[str[i] - 'a']++;
            curBackNode->count++;
            curBackNode = curBackNode->child[str[i] - 'a'];
        }else
        {
            curBackNode->prevWordCount[str[i] - 'a']++;
            curBackNode->count++;
            curBackNode->child[str[i] - 'a'] = new backword;
            curBackNode = curBackNode->child[str[i] - 'a'];
        }
    }
}

int findWordCount(string str)
{
    if(str[0] != '?')
    {
        word* curNode = &wordRoot[str.size()];
        if(curNode->count == 0)
            return 0;
        
        //다음 위치의 단어가 ?가 될때까지 탐색 이후 갯수 return
        for(int i = 0; i < str.size()-1; i++)
        {
            if(str[i+1] != '?')
            {
                if(curNode->child[str[i] - 'a'] != nullptr)
                    curNode = curNode->child[str[i] - 'a'];
                else
                    return 0;
            }else
                return curNode->nextWordCount[str[i] - 'a'];
        }
    }else{
        backword* curBackNode = &backwordRoot[str.size()];
        if(curBackNode->count == 0)
            return 0;
        //접두사와 접미사가 ?라면 return 0;
        if(str[str.size()-1] == '?')
        {
            return curBackNode->count;
        }
        //이전 위치의 단어가 ?가 될때까지 탐색 이후 갯수 return
        for(int i = str.size()-1; i > 0; i--)
        {
            if(str[i-1] != '?')
            {
                if(curBackNode->child[str[i] - 'a'] != nullptr)
                    curBackNode = curBackNode->child[str[i] - 'a'];
                else
                    return 0;
            }else
                return curBackNode->prevWordCount[str[i] - 'a'];
        }
    }
}

vector<int> solution(vector<string> words, vector<string> queries) {
    vector<int> answer;
    
    for(string str : words)
    {
        insertWord(str);
    }
    for(string str : queries)
    {
        answer.push_back(findWordCount(str));
    }
    
    return answer;
}
```