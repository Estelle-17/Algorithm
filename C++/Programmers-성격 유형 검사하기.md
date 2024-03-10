### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/118666
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%84%B1%EA%B2%A9-%EC%9C%A0%ED%98%95-%EA%B2%80%EC%82%AC%ED%95%98%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

string solution(vector<string> survey, vector<int> choices) {
    string answer = "";
    vector<int> result(4, 0);
    
    //값 계산
    for(int i = 0; i < survey.size(); i++)
    {
        if(survey[i][0] == 'R' || survey[i][0] == 'T')
        {
            if(choices[i] < 4){
                if(survey[i][0] == 'R')
                    result[0] -= 4 - choices[i];
                else
                    result[0] += 4 - choices[i];
            }else if(choices[i] > 4){
                if(survey[i][0] == 'R')
                    result[0] += choices[i] - 4;
                else
                    result[0] -= choices[i] - 4;
            }
        }
        else if(survey[i][0] == 'C' || survey[i][0] == 'F')
        {
            if(choices[i] < 4){
                if(survey[i][0] == 'C')
                    result[1] -= 4 - choices[i];
                else
                    result[1] += 4 - choices[i];
            }else if(choices[i] > 4){
                if(survey[i][0] == 'C')
                    result[1] += choices[i] - 4;
                else
                    result[1] -= choices[i] - 4;
            }
        }
        else if(survey[i][0] == 'J' || survey[i][0] == 'M')
        {
            if(choices[i] < 4){
                if(survey[i][0] == 'J')
                    result[2] -= 4 - choices[i];
                else
                    result[2] += 4 - choices[i];
            }else if(choices[i] > 4){
                if(survey[i][0] == 'J')
                    result[2] += choices[i] - 4;
                else
                    result[2] -= choices[i] - 4;
            }
        }
        else if(survey[i][0] == 'A' || survey[i][0] == 'N')
        {
            if(choices[i] < 4){
                if(survey[i][0] == 'A')
                    result[3] -= 4 - choices[i];
                else
                    result[3] += 4 - choices[i];
            }else if(choices[i] > 4){
                if(survey[i][0] == 'A')
                    result[3] += choices[i] - 4;
                else
                    result[3] -= choices[i] - 4;
            }
        }
    }
    //값에 맞는 알파벳 입력
    for(int i = 0; i < 4; i++)
    {
        if(i == 0)
            if(result[i] <= 0)
                answer += 'R';
            else
                answer += 'T';
        else if(i == 1)
            if(result[i] <= 0)
                answer += 'C';
            else
                answer += 'F';
        else if(i == 2)
            if(result[i] <= 0)
                answer += 'J';
            else
                answer += 'M';
        else
            if(result[i] <= 0)
                answer += 'A';
            else
                answer += 'N';
    }
    
    return answer;
}
```