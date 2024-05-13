### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42890
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%ED%9B%84%EB%B3%B4%ED%82%A4

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<vector<string>> relation) {
    int answer = 0;
    int columnSize = relation[0].size();
    int rowSize = relation.size();
    
    queue<vector<int>> keys;
    
    //검색을 위한 모든 경우의 수 계산
    vector<int> permutationArray(columnSize, 1);
    for(int num = 0; num < columnSize; num++)
    {
        permutationArray[num] = 0;
        vector<int> column_vec = permutationArray;
        vector<int> key;
        
        bool isNotKey;      
        do{
            isNotKey = false;
            key = vector<int>();
            
            //후보 키 생성
            for(int i = 0; i < column_vec.size(); i++)
            {
                if(column_vec[i] == 0)
                    key.push_back(i);
            }
            if(isNotKey)
                continue;
            
            for(int target = 0; target < key.size(); target++)
            {
                if(isNotKey)    break;
                for(int i = 0; i < rowSize; i++)
                {
                    if(isNotKey)    break;
                    for(int j = i+1; j < rowSize; j++)
                    {
                        if(isNotKey)    break;
                        //만약 튜플이 같을 경우 나머지 속성에서 판별 가능한지 확인
                        if(relation[i][key[target]].compare(relation[j][key[target]]) == 0)
                        {
                            bool temp = false;
                            for(int k = 0; k < key.size(); k++)
                            {
                                if(relation[i][key[k]].compare(relation[j][key[k]]) != 0)
                                {
                                    temp = true;
                                    break;
                                }
                            }
                            if(!temp)
                            {
                                isNotKey = true;
                            }
                        }
                    }
                }
            }
            
            if(!isNotKey)
            {                
                keys.push(key);
            }
            
            
        } while(next_permutation(column_vec.begin(), column_vec.end()));
    }
    
    //최소성 검사
    vector<int> currentVec;   
    while(!keys.empty())
    {
        vector<int> v;
        currentVec = keys.front();    keys.pop();
        int qSize = keys.size();
        for(int i = 0; i < qSize; i++)
        {
            v = keys.front(); keys.pop();
            if(!includes(v.begin(), v.end(), currentVec.begin(), currentVec.end()))
            {
                keys.push(v);
            }
        }
        answer++;
    }
    
    return answer;
}
```