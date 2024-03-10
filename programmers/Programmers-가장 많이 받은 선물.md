### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/258712
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EA%B0%80%EC%9E%A5-%EB%A7%8E%EC%9D%B4-%EB%B0%9B%EC%9D%80-%EC%84%A0%EB%AC%BC

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

int solution(vector<string> friends, vector<string> gifts) {
    int answer = 0;
    unordered_map<string, int> friendDic;   //친구들을 번호로 매기기 위한 map
    vector<int> giftIndex(friends.size());  //친구들이 선물을 주고받은 수(선물지수)
    vector<vector<int>> giftRecord(friends.size(), vector<int>(friends.size(), 0)); //특정 친구에게 선물을 준 횟수
    
    for(int i = 0; i < friends.size(); i++)
    {
        friendDic.emplace(friends[i], i);
    }
    //선물을 준 상황을 배열에 입력
    for(string str : gifts)
    {
        vector<string> v = split(str, ' ');
        giftRecord[friendDic[v[0]]][friendDic[v[1]]]++;
        giftIndex[friendDic[v[0]]]++;
        giftIndex[friendDic[v[1]]]--;
    }
    
    int cnt;
    for(int i = 0; i < friends.size(); i++)
    {
        cnt = 0;
        for(int j = 0; j < friends.size(); j++)
        {
            if(i == j)  //자기 자신과는 계산X
                continue;
            if(giftRecord[i][j] > giftRecord[j][i]) //i 친구가 j 친구보다 선물을 더 많이 줬을 경우
            {
                cnt++;
            }else if(giftRecord[i][j] == giftRecord[j][i])  //i 친구와 j 친구의 선물 준 횟수가 같을 경우
            {
                if(giftIndex[i] > giftIndex[j]) //선물 지수가 더 클 경우
                {
                    cnt++;
                }
            }
        }
        answer = max(answer, cnt);  //제일 큰 값 저장
    }
    
    return answer;
}
```