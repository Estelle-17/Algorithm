### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42894
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%B8%94%EB%A1%9D-%EA%B2%8C%EC%9E%84

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int n;
vector<vector<int>> temp;
vector<vector<int>> blockBoard;
vector<pair<int, int>> blockPos[201];
int blockType[5][3][2] = {{{-1, 0}, {0, -1}, {0, -1}},
                         {{-1, 0}, {-1, 0}, {0, 1}},
                         {{-1, 0}, {-1, 0}, {0, -1}},
                         {{-1, 0}, {0, 1}, {0, 1}},
                         {{-1, 0}, {0, -1}, {0, 2}}};
int dx[4] = {0, 0, 1, -1};
int dy[4] = {1, -1, 0, 0};

void findBlock(int curNumber, int x, int y) //블록 발견 및 등록
{
    temp[x][y] = 0;
    blockPos[curNumber].push_back(make_pair(x, y));
    for(int i = 0; i < 4; i++)
    {
        if(x+dx[i] < 0 || x+dx[i] > n-1 || y+dy[i] < 0 || y+dy[i] > n-1)
            continue;
        
        if(temp[x+dx[i]][y+dy[i]] == curNumber)
        {
            findBlock(curNumber, x+dx[i], y+dy[i]);           
        }
    }
}

void eraseBlock(int num)    //블록 삭제
{
    for(int i = 0; i < n; i++)
    {
        for(int j = 0; j < n; j++)
        {
            if(blockBoard[i][j] == num)
            {
                blockBoard[i][j] = 0;
            }
        }
    }
}

void checkBlackBlock()  //검은 블록 놓을 수 있는 곳을 체크
{
    for(int i = 0; i < n; i++)
    {
        for(int j = 0; j < n; j++)
        {
            if(blockBoard[j][i] <= 0)
            {
                blockBoard[j][i] = -1;
            }else
            {
                break;
            }
        }
    }
}

int solution(vector<vector<int>> board) {
    int answer = 0;
    n = board.size();
    blockBoard = board;
    
    temp = blockBoard;
    for(int i = 0; i < n; i++)
    {
        for(int j = 0; j < n; j++)
        {
            if(temp[i][j] != 0)
            {
                findBlock(temp[i][j], i, j);
            }
        }
    }
    //넣을 수 있는 검은 블록을 -1로 처리
    for(int i = 0; i < n; i++)
    {
        for(int j = 0; j < n; j++)
        {
            if(blockBoard[j][i] <= 0)
            {
                blockBoard[j][i] = -1;
            }else
            {
                break;
            }
        }
    }
    
    int blockCnt = 0;
    bool isBlockAble = true;
    while(isBlockAble)  //더 이상 블록을 제거할 수 없을 때까지 검색
    {
        isBlockAble = false;
        for(int i = 0; i <= 200; i++)
        {
            if(!blockPos[i].empty())
            {
                for(int j = 0; j < 5; j++)
                {
                    if(blockPos[i].empty()) //만약 블록을 지울 수 있는 경우를 찾아서 없앴을 경우 넘어감
                        break;
                    
                    //5가지의 블록에 해당하는 블록이 있을 경우
                    if(blockPos[i][0].first - blockPos[i][1].first == blockType[j][0][0] &&
                       blockPos[i][0].second - blockPos[i][1].second == blockType[j][0][1] &&
                       blockPos[i][1].first - blockPos[i][2].first == blockType[j][1][0] &&
                       blockPos[i][1].second - blockPos[i][2].second == blockType[j][1][1] &&
                       blockPos[i][2].first - blockPos[i][3].first == blockType[j][2][0] &&
                       blockPos[i][2].second - blockPos[i][3].second == blockType[j][2][1])
                    {
                        switch(j){  //검은 블록을 놓아서 지울 수 있을 경우 제거
                            case 0:
                                if(blockBoard[blockPos[i][0].first][blockPos[i][0].second+1] == -1 &&
                                   blockBoard[blockPos[i][0].first][blockPos[i][0].second+2] == -1)
                                {    
                                    answer++;
                                    blockPos[i].clear();
                                    eraseBlock(i);
                                    checkBlackBlock();
                                    isBlockAble = true;
                                }
                                break;
                            case 1:
                                if(blockBoard[blockPos[i][0].first][blockPos[i][0].second-1] == -1 &&
                                   blockBoard[blockPos[i][1].first][blockPos[i][1].second-1] == -1)
                                {    
                                    answer++;
                                    blockPos[i].clear();
                                    eraseBlock(i);
                                    checkBlackBlock();
                                    isBlockAble = true;
                                }
                                break;
                            case 2:
                                if(blockBoard[blockPos[i][0].first][blockPos[i][0].second+1] == -1 &&
                                   blockBoard[blockPos[i][1].first][blockPos[i][1].second+1] == -1)
                                {    
                                    answer++;
                                    blockPos[i].clear();
                                    eraseBlock(i);
                                    checkBlackBlock();
                                    isBlockAble = true;
                                }
                                break;
                            case 3:
                                if(blockBoard[blockPos[i][0].first][blockPos[i][0].second-1] == -1 &&
                                   blockBoard[blockPos[i][0].first][blockPos[i][0].second-2] == -1)
                                {    
                                    answer++;
                                    blockPos[i].clear();
                                    eraseBlock(i);
                                    checkBlackBlock();
                                    isBlockAble = true;
                                }
                                break;
                            case 4:
                                if(blockBoard[blockPos[i][0].first][blockPos[i][0].second-1] == -1 &&
                                   blockBoard[blockPos[i][0].first][blockPos[i][0].second+1] == -1)
                                {    
                                    answer++;
                                    blockPos[i].clear();
                                    eraseBlock(i);
                                    checkBlackBlock();
                                    isBlockAble = true;
                                }
                                break;
                        }
                    }
                }
            }
        }
    }
    
    return answer;
}
```