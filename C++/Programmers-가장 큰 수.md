### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42746
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B0%80%EC%9E%A5-%ED%81%B0-%EC%88%98

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

bool compare(string a, string b)
{
    //수가 같다면 정렬할 필요 없음
    if(a == b)
        return true;
    
    string str1 = a + b;
    string str2 = b + a;
    
    //맨 앞에 있는 수가 0인 경우는 제외시킴
    if(str1[0] == '0')
        return false;
    else if(str2[0] == '0')
        return true;
    
    int num1, num2;
    
    num1 = stoi(a + b);
    num2 = stoi(b + a);
    
    //내림차순으로 정렬
    return num1 > num2;
}

string solution(vector<int> numbers) {
    string answer = "";
    vector<string> vec;
    
    for(int n : numbers)
    {
        vec.push_back(to_string(n));
    }
    
    sort(vec.begin(), vec.end(), compare);
    
    for(string s : vec)
    {
        answer = answer + s;
    }
    
    //배열 내에 0밖에 없다면 정답은 0
    return answer[0] == '0' ? "0" : answer;
}
```