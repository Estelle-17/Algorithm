### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/92341
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%A3%BC%EC%B0%A8-%EC%9A%94%EA%B8%88-%EA%B3%84%EC%82%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

bool compare(vector<int> a, vector<int> b){
    return a[0] < b[0];
}

vector<int> solution(vector<int> fees, vector<string> records) {
    vector<int> answer;
    vector<vector<int>> result;
    unordered_map<int, vector<int>> cars;
    unordered_map<int, vector<int>>::iterator iter;
    
    for(int i = 0; i < records.size(); i++) //차량들의 입차, 출차 시간 정렬
    {
        int carNumber = stoi(records[i].substr(6, 4));
        int minute = stoi(records[i].substr(0, 2)) * 60 + stoi(records[i].substr(3, 2));
        iter = cars.find(carNumber);
        if(iter == cars.end()){
            vector<int> v;
            v.push_back(minute);
            cars.emplace(make_pair(carNumber, v));
        }else{
            iter->second.push_back(minute);
        }
    }
    
    for(auto it : cars) //입차, 출차 시간을 기준으로 누적 시간 계산 및 주차요금 계산
    {
        int carNumber = it.first;
        vector<int> vec = it.second;
        
        //출차가 한가지 없다면 23:59분 추가
        if(vec.size() % 2 == 1)
            vec.push_back(23 * 60 + 59);
        
        int time = 0;
        for(int i = 0; i < vec.size(); i += 2)
        {
            time += vec[i+1] - vec[i];
        }
        int price = 0;
        if(time <= fees[0])
            price += fees[1];
        else{
            time -= fees[0];
            price += fees[1];
            if(time % fees[2] != 0)
            {
                price += (time / fees[2] + 1) * fees[3];
            }else{
                price += (time / fees[2]) * fees[3];
            }
        }
        vector<int> v;
        v.push_back(carNumber);
        v.push_back(price);
        result.push_back(v);
    }
    //차량번호가 낮은 순으로 정렬
    sort(result.begin(), result.end(), compare);
    for(vector<int> n : result)
    {
        answer.push_back(n[1]);
    }
    
    return answer;
}
```