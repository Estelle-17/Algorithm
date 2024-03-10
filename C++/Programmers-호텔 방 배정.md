### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/64063
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%98%B8%ED%85%94-%EB%B0%A9-%EB%B0%B0%EC%A0%95

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<long long> solution(long long k, vector<long long> room_number) {
    vector<long long> answer;
    unordered_map<long long, long long> room;
    queue<long long> q;
    
    unordered_map<long long, long long>::iterator iter;
    for(int i = 0; i < room_number.size(); i++)
    {
        long long curNumber = room_number[i];   //원하는 방 번호
        iter = room.find(curNumber);
        if(iter == room.end())  //원하는 방 번호가 배정되어있는지 확인
        {
            //없을 경우 방에 배정시켜준 후 map컨테이너에 (방 번호 : 대신 들어가야 하는 방 번호)식으로 저장해줌
            room.emplace(make_pair(curNumber, curNumber+1));
        }else
        {
            while(iter != room.end())   //있을 경우 value값을 보며 들어갈 방 검색
            {
                q.push(curNumber);  //지나온 방의 value값도 함께 바꿔주기 위해 queue에 저장
                curNumber = iter->second;
                iter = room.find(curNumber);
            }
            room.emplace(make_pair(curNumber, curNumber+1));    //빈 방을 map컨테이너에 저장해줌
            while(!q.empty())   //지나온 방들의 value값 갱신
            {
                long long cur = q.front();  q.pop();
                room[cur] = curNumber+1;
            }
        }
        answer.push_back(curNumber);
    }
    
    return answer;
}
```