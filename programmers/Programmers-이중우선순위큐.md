### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42628
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%9D%B4%EC%A4%91%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84%ED%81%90

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

class LinkedlistQueue
{
    struct Node
    {
        int value;
        Node* prev;
        Node* next;
    };
    
public:
    Node* head;
    Node* tail;
    int size = 0;
    
    void enqueue(int num)
    {
        //아무 값이 들어있지 않으면 첫 노드를 추가한다.
        if(head == nullptr)
        {
            Node* n = new Node();
            n->value = num;
            head = n;
            tail = n;
            head->prev = head;
            head->next = tail;
            tail->prev = head;
            tail->next = tail;
            size++;
        }
        else
        {
            Node* n = new Node();
            n->value = num;
            //값이 head보다 작거나 tail 보다 크면 바로 넣어줌
            if(n->value <= head->value)
            {
                head->prev = n;
                n->next = head;
                n->prev = n;
                head = n;
            }
            else if(n->value >= tail->value)
            {
                tail->next = n;
                n->prev = tail;
                n->next = n;
                tail = n;
            }
            else
            {
                Node* curNode = head;
                //오름차순으로 노드를 정렬하기 위하여 앞뒤 값을 비교하여 노드를 넣음
                while(curNode != nullptr)
                {
                    if(curNode->value < num)
                    {
                        if(curNode->next == tail)
                        {
                            curNode->next = n;
                            n->prev = curNode;
                            n->next = tail;
                            tail->prev = n;
                            break;
                        }
                        else
                        {
                            curNode = curNode->next;
                        }
                    }
                    else
                    {
                        curNode->prev->next = n;
                        n->prev = curNode->prev;
                        curNode->prev = n;
                        n->next = curNode;
                        break;
                    }
                }
            }
            size++;
        }
        cout << endl;
    }
    
    void dequeue(int num)
    {
        if(size == 0)
        {
            return;
        }
        if(size == 1)
        {
            head = nullptr;
            tail = nullptr;
            size--;
            return;
        }
        if(num == 1)
        {
            tail = tail->prev;
            tail->next = tail;
        }
        else
        {
            head = head->next;
            head->prev = head;
        }
        size--;
    }
};

vector<int> solution(vector<string> operations) {
    vector<int> answer;
    LinkedlistQueue* linkedQueue = new LinkedlistQueue();
    for(string str : operations)
    {
        if(str[0] == 'I')
        {
            str = str.substr(2, str.size()-2);
            int num = stoi(str);
            linkedQueue->enqueue(num);
        }
        else
        {
            str = str.substr(2, str.size()-2);
            int num = stoi(str);
            linkedQueue->dequeue(num);
        }
    }
    if(linkedQueue->head == nullptr)
    {
        answer.push_back(0);
        answer.push_back(0);
    }
    else
    {
        answer.push_back(linkedQueue->tail->value);
        answer.push_back(linkedQueue->head->value);
    }

    return answer;
}
```