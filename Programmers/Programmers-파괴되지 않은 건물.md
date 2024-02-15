### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/92344
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%8C%8C%EA%B4%B4%EB%90%98%EC%A7%80-%EC%95%8A%EC%9D%80-%EA%B1%B4%EB%AC%BC

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<vector<int>> board, vector<vector<int>> skill) {
    int answer = 0;
    vector<vector<int>> building(board.size()+1, vector<int>(board[0].size()+1, 0));
    
    //건물의 공격과 회복을 배열에 저장해둠
    for(int idx = 0; idx < skill.size(); idx++)
    {
        if(skill[idx][0] == 1)
        {
            building[skill[idx][1]][skill[idx][2]] += -skill[idx][5];
            building[skill[idx][1]][skill[idx][4]+1] += skill[idx][5];
            building[skill[idx][3]+1][skill[idx][2]] += skill[idx][5];
            building[skill[idx][3]+1][skill[idx][4]+1] += -skill[idx][5];
        }else{
            building[skill[idx][1]][skill[idx][2]] += skill[idx][5];
            building[skill[idx][1]][skill[idx][4]+1] += -skill[idx][5];
            building[skill[idx][3]+1][skill[idx][2]] += -skill[idx][5];
            building[skill[idx][3]+1][skill[idx][4]+1] += skill[idx][5];
        }
    }
    //건물들이 부셔졌는지 확인
    for(int i = 0; i < board.size(); i++)
    {
        int sum = 0;
        for(int j = 0; j < board[i].size(); j++)
        {            
            sum += building[i][j];
            building[i][j] = sum;
            if(i != 0)
                building[i][j] += building[i-1][j];
            
            if(board[i][j] + building[i][j] >= 1)
                answer++;
        }
    }
    
    return answer;
}
```