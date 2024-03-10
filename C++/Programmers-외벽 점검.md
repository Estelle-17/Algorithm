### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/60062
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%99%B8%EB%B2%BD-%EC%A0%90%EA%B2%80

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int INF = 2100000000;

int solution(int n, vector<int> weak, vector<int> dist) {
    int answer = INF;
    int wallSize = weak.size();
    
    //원형탐색을 위한 값 추가
    for(int i = 0; i < wallSize; i++)
        weak.push_back(weak[i]+n);
    
    sort(dist.begin(), dist.end());
    
    do{
        for(int i = 0; i < wallSize; i++)
        {
            int startWall = weak[i];
            int endWall = weak[i+wallSize-1];
            
            for(int j = 0; j < dist.size(); j++)
            {
                //시작점 지정
                startWall += dist[j];
                
                if(startWall >= endWall)    //시작점과 도착점이 같으면 탐색 종료
                {
                    answer = min(answer, j+1);
                    break;
                }
                
                for(int k = i+1; k < wallSize+i; k++)   //현재 취약 벽을 찾는 친구보다 더 멀리 있는 벽이 나올때까지 검색
                {
                    if(weak[k] > startWall)
                    {
                        startWall = weak[k];
                        break;
                    }
                }
            }
            if(answer == 1)
                return answer;
        }
    } while(next_permutation(dist.begin(), dist.end()));    //모든 친구들의 경우의 수를 검색하기 위해 순열을 찾아 계산해줌
    
    if(answer == INF)
        answer = -1;
    
    return answer;
}
```