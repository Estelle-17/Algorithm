### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/43236
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%A7%95%EA%B2%80%EB%8B%A4%EB%A6%AC

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(int distance, vector<int> rocks, int n) {
    int answer = 0;
    
    sort(rocks.begin(), rocks.end());
    int high = distance;
    int low = 0;
    //돌 사이의 거리를 중심으로 잡고 이분탐색 시작
    while(low <= high){
        int mid = (high+low)/2;
        int curRock = 0;
        int deleteRockCount = 0;
        //시작지점부터 돌까지의 거리 계산
        for(int i = 0; i < rocks.size(); i++)
        {
            if(rocks[i]-curRock >= mid){    //mid값보다 클 경우 새로 갱신
                curRock = rocks[i];
            }else{  //mid값보다 작을 경우 바위 제거
                deleteRockCount++;
            }
            if(deleteRockCount > n)
                break;
        }
        //도착지점까지와의 거리 계산
        if(distance-curRock >= mid){
            curRock = distance;
        }else{
            deleteRockCount++;
        }
        
        if(deleteRockCount > n)
            high = mid-1;
        else
            low = mid+1;
    }
    answer = high;
    
    return answer;
}
```