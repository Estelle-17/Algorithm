### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/84021
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%8D%BC%EC%A6%90-%EC%A1%B0%EA%B0%81-%EC%B1%84%EC%9A%B0%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

#define peaceMaxSize 6

using namespace std;

int board[52][52];
int pieceTable[52][52];
int onePieceTable[peaceMaxSize][peaceMaxSize];
int boardSize;

vector<vector<pair<int, int>>> pieceHoles;
vector<vector<pair<int, int>>> pieces;
vector<pair<int, int>> v;

void makePieceHole(int x, int y)
{
    v.push_back(make_pair(x, y));
    board[x][y] = 1;
    if(board[x+1][y] == 0)
    {
        makePieceHole(x+1, y);
    }
    if(board[x-1][y] == 0)
    {
        makePieceHole(x-1, y);
    }
    if(board[x][y+1] == 0)
    {
        makePieceHole(x, y+1);
    }
    if(board[x][y-1] == 0)
    {
        makePieceHole(x, y-1);
    }
}

void makePiece(int x, int y, bool isOnePiece)
{
    v.push_back(make_pair(x, y));
    if(isOnePiece)
    {
        onePieceTable[x][y] = 0;
        if(onePieceTable[x+1][y] == 1)
        {
            makePiece(x+1, y, isOnePiece);
        }
        if(onePieceTable[x-1][y] == 1)
        {
            makePiece(x-1, y, isOnePiece);
        }
        if(onePieceTable[x][y+1] == 1)
        {
            makePiece(x, y+1, isOnePiece);
        }
        if(onePieceTable[x][y-1] == 1)
        {
            makePiece(x, y-1, isOnePiece);
        }
    }
    else
    {
        pieceTable[x][y] = 0;    
        if(pieceTable[x+1][y] == 1)
        {
            makePiece(x+1, y, isOnePiece);
        }
        if(pieceTable[x-1][y] == 1)
        {
            makePiece(x-1, y, isOnePiece);
        }
        if(pieceTable[x][y+1] == 1)
        {
            makePiece(x, y+1, isOnePiece);
        }
        if(pieceTable[x][y-1] == 1)
        {
            makePiece(x, y-1, isOnePiece);
        }
    }
}

vector<pair<int, int>> makePieceArray(vector<pair<int, int>> v)
{
    int xmin = 100;
    int ymin = 100;
    for(pair<int, int> p : v)
    {
        xmin = min(xmin, p.first);
        ymin = min(ymin, p.second);
    }
    for(int i = 0; i < v.size(); i++)
    {
        v[i].first -= xmin;
        v[i].second -= ymin;
    }
    
    return v;
}
//table의 조각들은 90도씩 돌려가며 모양을 찾아줌
void rotatePiece(vector<pair<int, int>> vec)
{
    v = vector<pair<int, int>>();
    vec = makePieceArray(vec);
    int arr[peaceMaxSize][peaceMaxSize] = {0,};
    int temp[peaceMaxSize][peaceMaxSize] = {0,};
    for(pair<int, int> p : vec)
    {
        temp[p.first][p.second] = 1;
    }
    for(int l = 0; l < 3; l++)  //90도씩 3번
    {         
        //도형을 돌려줌
        for(int i = 0; i < peaceMaxSize; i++)
        {
            for(int j = 0; j < peaceMaxSize; j++)
            {
                onePieceTable[i][j] = temp[peaceMaxSize-j-1][i];
                arr[i][j] = temp[peaceMaxSize-j-1][i];
            }
        }
        //이후 도형의 좌표 등록
        for(int i = 0; i < peaceMaxSize; i++)
        {
               for(int j = 0; j < peaceMaxSize; j++)
               {
                temp[i][j] = arr[i][j];
                if(onePieceTable[i][j] == 1)
                {
                    makePiece(i, j, true);
                    v = makePieceArray(v);
                    pieces.push_back(v);
                    v = vector<pair<int, int>>();
                }
            }
        }
    }
}

bool pairCompare(pair<int, int> a, pair<int, int> b)
{
    return a.first == b.first && a.second == b.second;
}

int solution(vector<vector<int>> game_board, vector<vector<int>> table) {
    int answer = 0;
    boardSize = game_board.size();
    
    //배열 초기화
    for(int i = 0; i < boardSize+2; i++)
    {
        for(int j = 0; j < boardSize+2; j++)
        {
            board[i][j] = 1;
            pieceTable[i][j] = 0;
        }
    }
    //조각을 찾기 위한 배열 제작
    for(int i = 0; i < boardSize; i++)
    {
        for(int j = 0; j < boardSize; j++)
        {
            board[i+1][j+1] = game_board[i][j];
            pieceTable[i+1][j+1] = table[i][j];
        }
    }
    //조각들 검색
    for(int i = 1; i <= boardSize; i++)
    {
        for(int j = 1; j <= boardSize; j++)
        {
            if(board[i][j] == 0)
            {
                makePieceHole(i, j);
                pieceHoles.push_back(v);
                v = vector<pair<int, int>>();
            }
            if(pieceTable[i][j] == 1)
            {
                makePiece(i, j, false);
                v = makePieceArray(v);
                pieces.push_back(v);
                rotatePiece(v);
                v = vector<pair<int, int>>();
            }
        }
    }


    //찾아낸 조각들의 형태를 가공해줌
    for(int i = 0; i < pieceHoles.size(); i++)
    {
        pieceHoles[i] = makePieceArray(pieceHoles[i]);
    }
    //조각을 넣는 모양과 조각들의 모양이 같은지 확인해줌
    int cnt = 0;
    for(int i = 0; i < pieceHoles.size(); i++)
    {
        for(int j = 0; j < pieces.size(); j++)
        {
            if(pieceHoles[i].size() == pieces[j].size() && !pieceHoles[i].empty() && !pieces[i].empty())
            {
                for(int k = 0; k < pieceHoles[i].size(); k++)
                {
                    if(pairCompare(pieceHoles[i][k], pieces[j][k]))
                    {
                        cnt++;
                    }
                    else
                    {
                        break;
                    }
                }
                if(cnt == pieceHoles[i].size())
                {
                    pieceHoles[i] = vector<pair<int, int>>();
                    int index = (j / 4) * 4;
                    for(int k = index; k < index+4; k++)
                    {
                        pieces[k] = vector<pair<int, int>>();
                        pieces[k].push_back(make_pair(-1, -1));
                    }
                    answer += cnt;
                }
                cnt = 0;
            }
        }
    }
    
    return answer;
}
```