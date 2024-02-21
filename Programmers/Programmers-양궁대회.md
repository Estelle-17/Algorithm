### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/92342
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-n1%EC%B9%B4%EB%93%9C%EA%B2%8C%EC%9E%84

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n, vector<int> info) {
    vector<int> answer;
    vector<int> arr(10, 0);
    vector<int> copyArr;
    vector<vector<int>> results;
    
    for(int i = 0; i < 10; i++)
    {
        arr[i] = 1;
        copyArr = arr;
        do{
            vector<int> v(12, 0);
            for(int j = 0; j < 10; j++) //어피치 점수와 라이언 점수 차이를 계산
            {
                if(copyArr[j] == 1)
                {
                    v[j] += info[j] + 1;
                    v[11] += info[j] + 1;
                }
            }
            if(v[11] <= n) //어피치 점수와 라이언 점수 차이를 계산 및 맞춘 화살의 갯수도 같이 계산
            {
                v[10] = n - v[11];
                results.push_back(v);
            }
        } while(next_permutation(copyArr.begin(), copyArr.end()));
    }
    for(vector<int> v : results)
    {
        cout << "[";
        for(int n : v)
        {
            cout << n << ",";
        }
        cout << "]" << endl;
    }
    
    return answer;
}
```