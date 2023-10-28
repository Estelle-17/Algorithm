### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/17685
## 사용언어 - C++

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

class Node
{
    public:
        Node* child[26] = {NULL,};
        bool isWord = false;
        int wordCount[26] = {0,};
};

Node root;

void insert(string s)
{
    Node* cur = &root;
    for(int i = 0; i < s.size(); i++)
    {
        if(cur->child[s[i] - 'a'] != NULL)
        {
            cur->wordCount[s[i] - 'a']++;
            cur = cur->child[s[i] - 'a'];
        }
        else
        {
            cur->wordCount[s[i] - 'a']++;
            cur->child[s[i] - 'a'] = new Node;
            cur = cur->child[s[i] - 'a'];
            
        }
    }
    cur->isWord = true;
}

int find(string s)
{
    Node* cur = &root;
    int count = 0;
    for(int i = 0; i < s.size(); i++)
    {
        count++;
        if(cur->wordCount[s[i] - 'a'] == 1)
        {
            return count;
        }
        cur = cur->child[s[i] - 'a'];
    }
    return count;
}

int solution(vector<string> words) {
    int answer = 0;
    for(string s : words)
    {
        insert(s);
    }
    
    for(string s : words)
    {
        answer += find(s);
    }
    
    return answer;
}
```