### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/12904
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B0%80%EC%9E%A5-%EA%B8%B4-%ED%8C%B0%EB%A6%B0%EB%93%9C%EB%A1%AC

```cpp
#include <bits/stdc++.h>
#include <iostream>
#include <string>
using namespace std;
int solution(string s)
{
    int answer = 1;
    int strSize = s.size();
    
    int cnt = 0;
    for(int i = 0; i < strSize-1; i++)
    {
        if(i > 0)
        {
            if(s[i-1] == s[i+1])
            {
                cnt += 3;
                int left = i-2;
                int right = i+2;
                while(left >= 0 && right < strSize)
                {
                    if(s[left] == s[right]){
                        cnt += 2;
                    }else{
                        break;
                    }
                    left--;
                    right++;
                }
                answer = max(answer, cnt);
            }
            cnt = 0;
        }
        if(s[i] == s[i+1])
        {
            cnt += 2;
            int left = i-1;
            int right = i+2;
            while(left >= 0 && right < strSize)
            {
                if(s[left] == s[right]){
                    cnt += 2;
                }else{
                    break;
                }
                left--;
                right++;
            }
            answer = max(answer, cnt);
        }
        cnt = 0;
    }

    return answer;
}
```