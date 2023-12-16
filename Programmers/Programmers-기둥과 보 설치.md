### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/60061
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EA%B8%B0%EB%91%A5%EA%B3%BC-%EB%B3%B4-%EC%84%A4%EC%B9%98

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<vector<int>> PillarMap;
vector<vector<int>> PillarBeamMap;

bool checkInstallEnable(int x, int y, int kind){
    if(kind == 0)
    {
        if(x == 0)
        {
            return true;
        }
        if(y > 0)
        {
            //설치할 기둥 밑에 기둥이 있는가
            //설치할 기둥 위치에 보가 한개라도 설치되어 있는가
            if(PillarBeamMap[x][y-1] > 0 || PillarBeamMap[x][y] > 0)
            {
                return true;
            }
        }
        if(PillarMap[x-1][y] > 0 || PillarBeamMap[x][y] > 0)
        {
            return true;
        }
    }else
    {
        //설치할 보 밑이나 오른쪽 밑에 기둥이 설치되어 있는가
        if(PillarMap[x-1][y] > 0 || PillarMap[x-1][y+1] > 0)
        {
            return true;
        }else if(y == 0)    //위 상황이 아니고 왼쪽 벽에 붙어있다면
        {
            return false;
        }
        //양쪽으로 보가 존재하는가
        if(PillarBeamMap[x][y-1] > 0 && PillarBeamMap[x][y+1] > 0)
        {
            return true;
        }
    }
    return false;
}

vector<vector<int>> solution(int n, vector<vector<int>> build_frame) {
    vector<vector<int>> answer;
    PillarMap.assign(n+1, vector<int>(n+1));
    PillarBeamMap.assign(n+1, vector<int>(n+1));
    
    for(int i = 0; i < build_frame.size(); i++)
    {
        int X = build_frame[i][0];
        int Y = build_frame[i][1];
        
        //기둥과 보 설치 및 제거
        if(build_frame[i][3] == 1)
        {
            //설치가 가능할 경우
            if(checkInstallEnable(Y, X, build_frame[i][2]))
            {
                build_frame[i][2] == 0 ? PillarMap[Y][X]++ : PillarBeamMap[Y][X]++;
            }
        }else
        {
            //위치의 기둥이나 보를 제외하고 구조물이 가능한지 확인
            build_frame[i][2] == 0 ? PillarMap[Y][X]-- : PillarBeamMap[Y][X]--;
            for(int j = 0; j < n+1; j++)
            {
                for(int k = 0; k < n+1; k++)
                {
                    if(PillarMap[k][j] > 0)
                    {
                        if(!checkInstallEnable(k, j, 0))
                        {
                            build_frame[i][2] == 0 ? PillarMap[Y][X]++ : PillarBeamMap[Y][X]++;
                            j = n+1;
                            k = n+1;
                            break;
                        }
                    }
                    if(PillarBeamMap[k][j] > 0)
                    {
                        if(!checkInstallEnable(k, j, 1))
                        {
                            build_frame[i][2] == 0 ? PillarMap[Y][X]++ : PillarBeamMap[Y][X]++;
                            j = n+1;
                            k = n+1;
                            break;
                        }
                    }
                }
            }
        }
    }
    
    vector<int> v;
    for(int i = 0; i < n+1; i++)
    {
        for(int j = 0; j < n+1; j++)
        {
            if(PillarMap[j][i] > 0)
            {
                v.push_back(i);
                v.push_back(j);
                v.push_back(0);
                answer.push_back(v);
                v.clear();
            }
            if(PillarBeamMap[j][i] > 0)
            {
                v.push_back(i);
                v.push_back(j);
                v.push_back(1);
                answer.push_back(v);
                v.clear();
            }
        }
    }
    
    return answer;
}
```