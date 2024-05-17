### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/60058
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B4%84%ED%98%B8-%EB%B3%80%ED%99%98

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

bool checkCorrectString(string str)
{
    int bracketCnt = 0;
    
    for(int i = 0; i < str.length(); i++) //올바른 괄호 문자열인지 확인
    {
        if(str[i] == '(')
        {
            bracketCnt++;
        }else
        {
            bracketCnt--;
        }
        if(bracketCnt < 0)
        {
            return true;
        }
    }
    return false;
}

string checkString(string w)
{
    //빈 문자열이면 빈 문자열 반환
    if(w == "")
        return "";
    
    string u = "";
    string v = "";
    pair<int, int> brackets;
    
    brackets = make_pair(0, 0);

    //균형잡힌 괄호 문자열 탐색
    for(int i = 0; i < w.length(); i++)
    {
        if(w[i] == '(')
        {
            brackets.first++;
        }else
        {
            brackets.second++;
        }
        u = u + w[i];
        
        if(brackets.first != 0 && brackets.second != 0 && brackets.first == brackets.second)
        {
            v = w.substr(i+1);
            break;
        }
    }
    
    if(checkCorrectString(u))
    {
        string str = "(" + checkString(v) + ")";
        //첫번째와 마지막 문자를 제거한 문자열 생성
        string s = u.substr(1, u.length()-2);
        //이후 (와 )를 바꿔줌
        for(int i = 0; i < s.length(); i++)
        {
            if(s[i] == '(')
                s[i] = ')';
            else
                s[i] = '(';
        }
        str = str + s;
        return str;
        
    }else{
        return u + checkString(v);
    }
}

string solution(string p) {
    string answer = "";
    string str = "";
    
    answer = checkString(p);
    
    return answer;
}
```