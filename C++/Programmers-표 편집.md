### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/81303
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%91%9C-%ED%8E%B8%EC%A7%91

```cpp

#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

struct Node{
    int index;
    int prev;
    int next;
    Node(int n, int a, int b) : index(n), prev(a), next(b)  {}
};

string solution(int n, int k, vector<string> cmd) {
    string answer(n, 'O');
    stack<int> cutNodeNum;
    vector<Node> table;
    
    //노드 연결
    for(int i = 0; i < n; i++)
    {
        table.push_back(Node(i, i-1, i+1));
    }
    table[0].prev = -1;
    table[n-1].next = -1;
    
    int curIndex = k;
    for(int i = 0; i < cmd.size(); i++)
    {
        //값만큼 앞의 노드로 이동
        if(cmd[i][0] == 'U')
        {
            int num = stoi(cmd[i].substr(2, cmd[i].size()-2));
            while(num > 0)
            {
                num--;
                curIndex = table[curIndex].prev;
            }
        }
        //값만큼 뒤의 노드로 이동
        else if(cmd[i][0] == 'D')
        {
            int num = stoi(cmd[i].substr(2, cmd[i].size()-2));
            while(num > 0)
            {
                num--;
                curIndex = table[curIndex].next;
            }
        }
        else if(cmd[i][0] == 'C')   //노드 삭제
        {
            //복구작업을 위해 값 저장
            cutNodeNum.push(curIndex);
            Node n = table[curIndex];
            if(n.prev == -1)
            {
                table[n.next].prev = -1;
                curIndex = table[n.next].index;
            }else if(n.next == -1)
            {
                table[n.prev].next = -1;
                curIndex = table[n.prev].index;
            }else
            {
                table[n.prev].next = n.next;
                table[n.next].prev = n.prev;
                curIndex = table[n.next].index;
            }
            answer[n.index] = 'X';
        }else   //삭제를 진행했던 노드를 순서대로 다시 연결
        {
            Node n = table[cutNodeNum.top()];
            if(n.prev == -1)
            {
                table[n.next].prev = n.index;
            }else if(n.next == -1)
            {
                table[n.prev].next = n.index;
            }else
            {
                table[n.prev].next = n.index;
                table[n.next].prev = n.index;
            }
            cutNodeNum.pop();
            answer[n.index] = 'O';
        }
    }
    
    return answer;
}
```