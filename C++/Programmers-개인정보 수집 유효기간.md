### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/150370
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B0%9C%EC%9D%B8%EC%A0%95%EB%B3%B4-%EC%88%98%EC%A7%91-%EC%9C%A0%ED%9A%A8%EA%B8%B0%EA%B0%84

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

bool checkDay(string date, string personalDate, int addMonth){
    int index = 0;
    vector<int> curDate;
    vector<int> persDate;
    string str1 = "";
    string str2 = "";
    //년, 월, 일 따로 나눠줌
    while(index < date.size())
    {
        if(date[index] != '.'){
            str1 = str1 + date[index];
            str2 = str2 + personalDate[index];
        }else{
            curDate.push_back(stoi(str1));
            persDate.push_back(stoi(str2));
            str1 = "";
            str2 = "";
        }
        index++;
    }
    curDate.push_back(stoi(str1));
    persDate.push_back(stoi(str2));
    persDate[1] += addMonth;
    
    while(persDate[1] > 12)
    {
        persDate[0]++;
        persDate[1] -= 12;
    }
 
    //만약 현재날짜보다 더 전의 날짜를 가리키면 true
    if(curDate[0] > persDate[0] ||
       curDate[0] == persDate[0] && curDate[1] > persDate[1] ||
       curDate[0] == persDate[0] && curDate[1] == persDate[1] && curDate[2] >= persDate[2])
        return true;
    
    return false;
}

vector<int> solution(string today, vector<string> terms, vector<string> privacies) {
    vector<int> answer;
    unordered_map<string, int> term;
    //약관에 따른 저장하는 유효기간의 값 저장
    for(string str : terms)
    {
        int num = stoi(str.substr(2, str.size() - 2));
        term.emplace(make_pair(str.substr(0, 1), num));
    }
    //앞에서부터 만료됬는지 확인
    for(int i = 0; i < privacies.size(); i++)
    {
        int month = term[privacies[i].substr(11, 1)];
        if(checkDay(today, privacies[i], month))
        {
            answer.push_back(i+1);
        }
    }
    
    return answer;
}
```