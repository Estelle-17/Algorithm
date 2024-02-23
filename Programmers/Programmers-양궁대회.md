### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/92342
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%96%91%EA%B6%81%EB%8C%80%ED%9A%8C

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n, vector<int> info) {
    vector<int> answer;
    vector<int> arr(10, 1);
    vector<int> copyArr;
    vector<int> result;
    
    arr[0] = 0;
    for(int i = 0; i < 10; i++)
    {
        arr[i] = 0;
        copyArr = arr;
        do{
            vector<int> v(13, 0);
            for(int j = 0; j < 10; j++) //어피치 점수와 라이언 점수 차이 및 과녁에 맞춘 화살 계산
            {
                if(copyArr[j] == 0)
                {
                    v[j] += info[j] + 1;
                    v[11] += info[j] + 1;
                    v[12] += 10 - j;
                }else if(copyArr[j] == 1 && info[j] != 0){
                    v[12] -= 10 - j;
                }
            }
            if(v[11] <= n && v[12] >= 0) //점수 차가 더 큰 경우 검색
            {
                v[10] = n - v[11];
                v[11] += v[10];
                if(result.empty() || result[12] < v[12])
                    result = v;
                else if(result.empty() || result[12] == v[12])  //만약 점수가 같다면 더 적은 점수를 맞췄는지 확인
                {
                    for(int j = 9; j >= 0; j--)
                    {
                        if(result[j] < v[j]){
                            result = v;
                            break;
                        }else if(result[j] > v[j])
                            break;
                    }
                }
            }
        } while(next_permutation(copyArr.begin(), copyArr.end()));
    }
    
    if(result.size() == 0 || result[12] == 0)   //무승부거나 값이 없을 때 -1
        answer.push_back(-1);
    else
        for(int j = 0; j <= 10; j++)
        {
            answer.push_back(result[j]);
        }
    
    return answer;
}
```