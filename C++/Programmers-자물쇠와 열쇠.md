### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/60059
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%9E%90%EB%AC%BC%EC%87%A0%EC%99%80-%EC%97%B4%EC%87%A0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int keySize;
int lockSize;
bool answer;
vector<vector<int>> keyTile;
vector<vector<int>> lockTile;

void solve(int startX, int startY)
{
    //자물쇠 사이즈만큼 비교하여 열쇠가 맞는지 확인
    for(int i = startX; i < startX + lockSize; i++)
    {
        for(int j = startY; j < startY + lockSize; j++)
        {
            if(keyTile[i][j] != lockTile[i - startX][j - startY])
            {
                return;
            }
        }
    }
    answer = true;
}

bool solution(vector<vector<int>> key, vector<vector<int>> lock) {
    answer = false;
    keySize = key.size();
    lockSize = lock.size();
    lockTile = lock;
    //열쇠크기 주변에 자물쇠 크기만큼 추가로 배열을 늘려줌
    keyTile.assign(lockSize*2+keySize, vector<int>(lockSize*2+keySize, 0));
    
    //자물쇠와 열쇠를 더 좋게 맞추기 위해 0과 1을 바꿔줌
    for(int i = 0; i < lockSize; i++)
    {
        for(int j = 0; j < lockSize; j++)
        {
            if(lockTile[i][j] == 1)
                lockTile[i][j] = 0;
            else
                lockTile[i][j] = 1;
        }
    }
    //열쇠 사이즈 입력
    for(int i = 0; i < keySize; i++)
    {
        for(int j = 0; j < keySize; j++)
        {
            keyTile[i+lockSize][j+lockSize] = key[i][j];
        }
    }
    
    //90도씩 돌려가며 열쇠 모양 찾기
    for(int k = 0; k < 4; k++)
    {
        for(int i = 1; i < keySize+lockSize; i++)
        {
            for(int j = 1; j < keySize+lockSize; j++)
            {
                solve(i, j);
                if(answer)
                {
                    return answer;
                }
            }
        }
        //열쇠 90도 돌리기
        vector<vector<int>> temp = keyTile;
        for(int i = 0; i < lockSize*2+keySize; i++)
        {
            for(int j = 0; j < lockSize*2+keySize; j++)
            {
                keyTile[i][j] = temp[lockSize*2+keySize-1-j][i];
            }
        }
    }
    
    return answer;
}
```