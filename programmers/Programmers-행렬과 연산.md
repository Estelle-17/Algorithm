### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/118670
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%96%89%EB%A0%AC%EA%B3%BC-%EC%97%B0%EC%82%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<vector<int>> solution(vector<vector<int>> rc, vector<string> operations) {
    int xSize = rc[0].size();
    int ySize = rc.size();
    vector<vector<int>> answer;
    deque<deque<int>*> dq;  //포인터로 선언하여 Deep Copy가 아닌 Shallow Copy가 일어나도록 함
    deque<int> dqTemp[ySize];   //포인터로 선언하기 위한 2차원배열의 주소값 생성
    //0번 index : 왼쪽, 1번 index : 오른쪽
    vector<deque<int>> sidedq(2);
    
    for(int i = 0; i < ySize; i++)
    {
        sidedq[0].push_back(rc[i][0]);
        sidedq[1].push_back(rc[i][rc[i].size()-1]);
        //rc[i].begin()+1부터 rc[i].begin()+xSize-1까지의 값을 원하는 곳에 복사함
        copy(rc[i].begin()+1, rc[i].begin()+xSize-1, inserter(dqTemp[i], dqTemp[i].end()));
        dq.push_back(&dqTemp[i]);
    }
    
    for(int i = 0; i < operations.size(); i++)
    {
        //맨 아래 있는 행을 맨 위로 옮겨줌
        if(operations[i] == "ShiftRow")
        {
            dq.push_front(dq.back());
            dq.pop_back();
            sidedq[0].push_front(sidedq[0].back());
            sidedq[0].pop_back();
            sidedq[1].push_front(sidedq[1].back());
            sidedq[1].pop_back();
        }else   //바깥쪽 원소들을 시계방향으로 회전
        {
            dq.back()->push_back(sidedq[1].back());
            sidedq[1].pop_back();
            sidedq[0].push_back(dq.back()->front());
            dq.back()->pop_front();
            dq.front()->push_front(sidedq[0].front());
            sidedq[0].pop_front();
            sidedq[1].push_front(dq.front()->back());
            dq.front()->pop_back();
        }
    }
    
    vector<int> v;
    for(int i = 0; i < ySize; i++)
    {
        v.push_back(sidedq[0][i]);
        for(int j : *dq.front())
        {
            v.push_back(j);
        }
        dq.pop_front();
        v.push_back(sidedq[1][i]);
        answer.push_back(v);
        v.clear();
    }
    
    return answer;
}
```