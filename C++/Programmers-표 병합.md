### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/150366
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%91%9C-%EB%B3%91%ED%95%A9

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<string> split(string input, char dlim)
{
	vector<string> result;

	stringstream ss;
	string stringBuffer;
	ss.str(input);
	
	while (getline(ss, stringBuffer, dlim))	
	{
		result.push_back(stringBuffer);
	}

	return result;
}

vector<string> solution(vector<string> commands) {
    vector<string> answer;
    vector<vector<pair<string, pair<int, int>>>> tableMap(51, vector<pair<string, pair<int, int>>>(51));
    
    //테이블의 기본값 설정
    for(int i = 1; i <= 50; i++)
    {
        for(int j = 1; j <= 50; j++)
        {
            tableMap[i][j].first = "";
            tableMap[i][j].second = make_pair(i, j);
        }
    }
    for(string str : commands)
    {
        vector<string> result = split(str, ' ');    //" "를 기준으로 문자열을 나눠줌
        if(result[0][0] == 'U' && result[0][1] == 'P'){
            if(result.size() == 4){ //단어를 셀 위치에 넣어줌
                int x = stoi(result[1]);
                int y = stoi(result[2]);
                //병합된 셀일 경우 답을 적어둔 셀 위치에 값을 넣어줌
                pair<int, int> p = make_pair(tableMap[x][y].second.first, tableMap[x][y].second.second);
                tableMap[x][y].first = result[3];
                tableMap[p.first][p.second].first = result[3];
            }else{  //주어진 단어와 같은 단어들을 전부 변환
                for(int i = 1; i <= 50; i++)
                {
                    for(int j = 1; j <= 50; j++)
                    {
                        if(tableMap[i][j].first == result[1])
                            tableMap[i][j].first = result[2];
                    }
                }
            }
        }else if(result[0][0] == 'M'){  //셀끼리 합침
            int x1 = stoi(result[1]);
            int y1 = stoi(result[2]);
            int x2 = stoi(result[3]);
            int y2 = stoi(result[4]);
            if(x1 == x2 && y1 == y2)    //두 좌표가 같으면 무시
                continue;
            pair<int, int> p;
            pair<int, int> changePair;
            
            //병합한 모든 셀들의 값은 첫 번째 좌표에 있는 값으로 통일시킴
            //만약 두 번째 좌표에만 값이 있을 시 두 번째 값으로 통일시킴
            //병합된 것을 알기 위해 값으로 통일시킨 값의 셀 위치를 각각 저장시킴
            string s;
            if(tableMap[x1][y1].first == "" && tableMap[x2][y2].first != "")
            {    
                p = make_pair(tableMap[x2][y2].second.first, tableMap[x2][y2].second.second);
                changePair = make_pair(tableMap[x1][y1].second.first, tableMap[x1][y1].second.second);
            }else{
                p = make_pair(tableMap[x1][y1].second.first, tableMap[x1][y1].second.second);
                changePair = make_pair(tableMap[x2][y2].second.first, tableMap[x2][y2].second.second);
            }
            
            for(int i = 1; i <= 50; i++)
            {
                for(int j = 1; j <= 50; j++)
                {
                    if(tableMap[i][j].second.first == changePair.first && tableMap[i][j].second.second == changePair.second)
                    {
                        tableMap[i][j].second = p;
                        tableMap[i][j].first = tableMap[p.first][p.second].first;
                    }
                }
            }
            
        }else if(result[0][0] == 'U' && result[0][1] == 'N'){   //병합된 셀 해제
            int x = stoi(result[1]);
            int y = stoi(result[2]);
            pair<int, int> p = tableMap[x][y].second;
            string s = tableMap[p.first][p.second].first;
            //지금 위치의 셀에서 저장해준 병합된 셀의 위치와 같은 모든 셀들을 전부 해제시킴
            //값은 지금 위치의 셀에 저장
            for(int i = 1; i <= 50; i++)
            {
                for(int j = 1; j <= 50; j++)
                {
                    if(tableMap[i][j].second.first == p.first && tableMap[i][j].second.second == p.second)
                    {
                        tableMap[i][j].first = "";
                        tableMap[i][j].second = make_pair(i, j);
                    }
                }
            }
            tableMap[x][y].first = s;
        }else if(result[0][0] == 'P'){  //값 출력
            int x = stoi(result[1]);
            int y = stoi(result[2]);
            if(tableMap[tableMap[x][y].second.first][tableMap[x][y].second.second].first == "")
                answer.push_back("EMPTY");
            else
                answer.push_back(tableMap[tableMap[x][y].second.first][tableMap[x][y].second.second].first);
        }
    }
    
    return answer;
}
```