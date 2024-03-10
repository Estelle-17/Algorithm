### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/77886
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-110-%EC%98%AE%EA%B8%B0%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<string> solution(vector<string> s) {
    vector<string> answer;
    
    for(int i = 0; i < s.size(); i++)
    {
        string curString = s[i];
        int oneCount = 0;
        int strCount = 0;
        int zeroIndex = -1;
        //110을 제외한 문자열을 저장해줌
        string result = "";
        //110을 찾아주며 뽑고 남은 문자열들과 뒤의 1의 갯수를 저장해줌
        for(int j = 0; j < curString.size(); j++)
        {
            if(curString[j] == '1')
            {
                oneCount++;
            }
            else
            {
                if(oneCount >= 2)
                {
                    oneCount -= 2;
                    strCount++;
                }
                else
                {
                    zeroIndex = j;
                    if(oneCount == 1)
                    {
                        result += "1";
                    }
                    oneCount = 0;                   
                    result += curString[j];
                }
            }
        }
        string temp = "";
        //110수만큼 110문자열을 나열하여 만듬
        while(strCount--)
        {
            temp += "110";
        }
        //110을 뺀 문자열에 0이 없는 경우
        if(zeroIndex == -1)
        {
            while(oneCount--)
            {
                result += "1";
            }
            result = temp + result;
        }
        else //110을 뺀 문자열에 0이 있는 경우 0 뒤에 110문자열을 삽입해줌
        {
            result = result + temp;
            while(oneCount--)   //1갯수만큼 맨 뒤에 추가
            {
                result = result + "1";
            }
        }
        
        answer.push_back(result);
    }
    
    return answer;
}
```