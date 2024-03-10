### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/43163
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%8B%A8%EC%96%B4-%EB%B3%80%ED%99%98

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int changeCount = 0;
int wordSize = 0;

void solve(string cur, string target, queue<string> words, int cnt)
{
    if(cur.compare(target) == 0)
    {
        if(changeCount > cnt || changeCount == 0)
        {
            changeCount = cnt;
        }
        return;
    }
    int size = words.size();
    for(int i = 0; i < size; i++)
    {
        //현재 체크할 단어를 뽑아줌
        string checkWord = words.front();
        words.pop();
        int difCount = 0;
        //앞에서부터 한 단어씩 비교해줌
        for(int j = 0; j < wordSize; j++)
        {
            if(cur[j] != checkWord[j])
            {
                difCount++;
            }
        }
        //만약 한개의 단어만 다르다면 단어를 들고 함수 실행
        if(difCount == 1)
        {
            solve(checkWord, target, words, cnt+1);
        }
        //다음 단어들을 위해 빼두었던 단어를 다시 넣어줌
        words.push(checkWord);
        difCount = 0;
    }
}

int solution(string begin, string target, vector<string> words) {
    int answer = 0;
    queue<string> q;
    for(string s : words)
    {
        q.push(s);
    }
    wordSize = begin.size();
    solve(begin, target, q, 0);
    
    answer = changeCount;
    
    return answer;
}
```