### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/68646
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%92%8D%EC%84%A0-%ED%84%B0%ED%8A%B8%EB%A6%AC%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> a) {
    int answer = 0;
    int left[1000000] = {0,};
    int right[1000000] = {0,};
    left[0] = 1000000001;
    right[a.size()-1] = 1000000001;

    //왼쪽과 오른쪽에서부터 제일 작은 숫자를 찾아 배열에 입력
    for(int i = 1; i < a.size(); i++)
    {
        if(left[i-1] > a[i-1])
        {
            left[i] = a[i-1];
        }
        else
        {
            left[i] = left[i-1];
        }
        
        if(right[a.size()-i] > a[a.size()-i])
        {
            right[a.size()-1-i] = a[a.size()-i];
        }
        else
        {
            right[a.size()-1-i] = right[a.size()-i];
        }
    }
    //비교할 풍선보다 작은수가 더 클 경우 남기는 것이 가능
    for(int i = 0; i < a.size(); i++)
    {
        if(left[i] > a[i] || right[i] > a[i])
        {
            answer++;
        }
    }
    
    return answer;
}
```