### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/87391
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B3%B5-%EC%9D%B4%EB%8F%99-%EC%8B%9C%EB%AE%AC%EB%A0%88%EC%9D%B4%EC%85%98

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

long long solution(int n, int m, int x, int y, vector<vector<int>> queries) {
    long long answer = 0;
    //범위 지정
    pair<long long, long long> start = make_pair(x, y);
    pair<long long, long long> end = make_pair(x, y);
    
    for(int i = queries.size()-1; i >= 0; i--)
    {
        long long dir = queries[i][0];
        long long dist = queries[i][1];
        
        switch(dir){
            case 0: //열이 감소하는 방향 -> 오른쪽으로 범위 이동
                if(start.second != 0)   //0이 왼쪽에 붙어있음 -> 벽의 부딪힘 때문에 dist값만큼의 오른쪽 공의 수를 0으로 옮길 수 있음
                    start.second += dist;
                
                end.second += dist; //범위 더해준 후 값 넘어가지 않도록 조정
                if(end.second > m-1)
                    end.second = m-1;
                break;
            case 1: //열이 중가하는 방향 -> 왼쪽으로 범위 이동
                start.second -= dist;
                if(start.second < 0)
                    start.second = 0;
                
                if(end.second != m-1)   //0이 오른쪽에 붙어있음 -> 벽의 부딪힘 때문에 dist값만큼의 왼쪽 공의 수를 m-1으로 옮길 수 있음
                    end.second -= dist;
                break;
            case 2: //행이 감소하는 방향 -> 아래쪽으로 범위 이동
                if(start.first != 0)    //0이 위쪽에 붙어있음 -> 벽의 부딪힘 때문에 dist값만큼의 아래쪽 공의 수를 0으로 옮길 수 있음
                    start.first += dist;
                
                end.first += dist;
                if(end.first > n-1)
                    end.first = n-1;
                break;
            case 3: //행이 증가하는 방향 -> 위쪽으로 범위 이동
                start.first -= dist;
                if(start.first < 0)
                    start.first = 0;
                
                if(end.first != n-1)    //0이 아래쪽에 붙어있음 -> 벽의 부딪힘 때문에 dist값만큼의 위쪽 공의 수를 n-1으로 옮길 수 있음
                    end.first -= dist;
                break;
        }
        //값이 지정된 격자를 넘어간다면 불가능한 조건
        if(start.first < 0 || start.second > m-1 || end.first < 0 || end.second > m-1)
            return answer;
    }
    //사각형의 범위 계산
    answer = (abs(start.first - end.first)+1) * (abs(start.second - end.second)+1);
    
    return answer;
}
```