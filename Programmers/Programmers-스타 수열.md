### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/70130
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%8A%A4%ED%83%80-%EC%88%98%EC%97%B4

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

bool compare(pair<int, int> a, pair<int, int> b)
{
    return a.second > b.second;
}

int solution(vector<int> a) {
    int answer = -1;
    vector<int> arr(a.size()+1, 0);
    vector<pair<int, int>> numCount;
    //숫자들의 갯수를 저장 후 큰 순서대로 정렬
    for(int n : a)
    {
        arr[n]++;
    }
    for(int i = 0; i < arr.size(); i++)
    {
        if(arr[i] != 0) numCount.push_back(make_pair(i, arr[i]));
    }
    sort(numCount.begin(), numCount.end(), compare);
    
    int maxCount = 0;
    for(int i = 0; i < numCount.size(); i++)
    {
        int result = 0;
        //나온 결과보다 더 작은 갯수를 가지면 계산할 필요X
        if(maxCount <= numCount[i].second)
        {
            for(int j = 0; j < a.size()-1; j++)
            {
                //현재 인덱스와 그 앞의 수가 다르며
                if(a[j] != a[j+1])
                {
                    //둘중 하나라도 교집합의 원소가 포함되있으면
                    if(numCount[i].first == a[j] || numCount[i].first == a[j+1])
                    {
                        result++;
                        j++;
                    }
                }
            }
        }
        maxCount = max(maxCount, result);
    }
    answer = maxCount * 2;
    
    return answer;
}
```