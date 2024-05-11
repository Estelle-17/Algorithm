### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42892
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B8%B8-%EC%B0%BE%EA%B8%B0-%EB%AC%B8%EC%A0%9C

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

class Node
{
    public:
        Node *left = NULL;
        Node *right = NULL;
        int num;
        int x;
};

vector<vector<int>> answer;

bool compare(vector<int> a, vector<int> b)
{
    if(a[1] == b[1])
        return a[0] < b[0];
    else
        return a[1] > b[1];
}

void makeTree(Node* root, Node* checkNode)
{
    //x를 기준으로 왼쪽 값이 없으면 추가, 있으면 이동 후 체크
    if(root->x > checkNode->x)
    {
        if(root->left == NULL)
        {
            root->left = checkNode;
        }else
        {
            makeTree(root->left, checkNode);
        }
    }
    else    //x를 기준으로 오른쪽 값이 없으면 추가, 있으면 이동 후 체크
    {
        if(root->right == NULL)
        {
            root->right = checkNode;
        }else
        {
            makeTree(root->right, checkNode);
        }
    }
}
void SetPreorder(Node* n)
{
    answer[0].push_back(n->num);
    if(n->left != NULL)
    {
        SetPreorder(n->left);
    }
    if(n->right != NULL)
    {
        SetPreorder(n->right);
    }
}
void SetPostorder(Node* n)
{
    if(n->left != NULL)
    {
        SetPostorder(n->left);
    }
    if(n->right != NULL)
    {
        SetPostorder(n->right);
    }
    answer[1].push_back(n->num);
}

vector<vector<int>> solution(vector<vector<int>> nodeinfo) {
    vector<Node> tree;
    
    //배열에 index번호 삽입
    for(int i = 0; i < nodeinfo.size(); i++)
    {
        nodeinfo[i].push_back(i+1);
    }
    
    //1번으로 y값으로 내림차순, 같을 경우 x값으로 오름차순 정렬
    sort(nodeinfo.begin(), nodeinfo.end(), compare);
    
    //node 생성
    for(int i = 0;i < nodeinfo.size(); i++)
    {
        Node n;
        n.num = nodeinfo[i][2];
        n.x = nodeinfo[i][0];
        tree.push_back(n);
    }
    
    //tree생성
    for(int i = 1;i < tree.size(); i++)
    {
        makeTree(&tree[0], &tree[i]);
    }
    
    //전위순회, 후위순회로 저장
    answer.assign(2, vector<int>());
    SetPreorder(&tree[0]);
    SetPostorder(&tree[0]);
    
    return answer;
}
```