### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/150368
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%9D%B4%EB%AA%A8%ED%8B%B0%EC%BD%98-%ED%95%A0%EC%9D%B8%ED%96%89%EC%82%AC

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

pair<int, int> maxResult;	//제일 큰 값 저장
pair<int, int> result;
vector<vector<int>> emoticon;
vector<int> emoticonPrice;	//할인율이 적용된 가격 저장
vector<vector<int>> user;

void solve(int index) {
    if(index >= emoticon.size())
        return;
        
    for(int i = 40; i >= 10; i -= 10)   //각 이모티콘마다 40~10%의 할인율을 적용시키면서 모든 경우의 수 계산
    {
        emoticon[index][0] = i;
        emoticonPrice[index] = emoticon[index][1] - ((emoticon[index][1] / 100) * emoticon[index][0]);  //할인율 적용 가격 저장
        if(index == emoticon.size()-1)
        {
            //이모티콘 플러스 서비스 가입 수와 이모티콘 매출액 계산
            result = make_pair(0, 0);
            for(vector<int> v : user)
            {
                int sum = 0;
                for(int j = 0; j < emoticonPrice.size(); j++)
                {
                    if(v[0] <= emoticon[j][0]){
                        sum += emoticonPrice[j];
                    }
                    if(sum >= v[1]){
                        result.first++;
                        sum = 0;
                        break;
                    }
                }
                result.second += sum;
            }
            if(maxResult.first < result.first ||
               maxResult.first == result.first && maxResult.second < result.second)
            {
                maxResult = result;
            }
        }else{
            solve(index+1);
        }
    }
}

vector<int> solution(vector<vector<int>> users, vector<int> emoticons) {
    vector<int> answer(2, 0);
    emoticonPrice.assign(emoticons.size(), 0);
    //이모티콘 할인율을 저장할 배열 생성
    for(int n : emoticons)
    {
        emoticon.push_back({0, n});
    }
    user = users;
    
    solve(0);
    
    answer[0] = maxResult.first;
    answer[1] = maxResult.second;
    
    return answer;
}
```