### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/340210
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%88%98%EC%8B%9D-%EB%B3%B5%EC%9B%90%ED%95%98%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int ChangeBinaryToNumber(int binary, string str)
{
    int sum = 0;
    int t = 1;
    for(int i = str.length()-1; i >= 0; i--)
    {
        sum += t * ((int)str[i] - 48);
        t *= binary;
    }
    return sum;
}
string ChangeNumberToBinary(int binary, int num)
{
    if(num == 0)
        return "0";
    
    string str = "";
    int t = binary * binary;  
    for(int i = 0; i < 3; i++)
    {
        int n = num / t;
        if(n != 0 || n == 0 && str != "")
            str = str + to_string(n);
        num = num % t;        
        t /= binary;
    }
    return str;
}

vector<string> solution(vector<string> expressions) {
    vector<string> answer;
    vector<vector<string>> str(100, vector<string>());
    int index = 0;
    vector<int> binaryNumbers;
    char maxNum = 2;
    
    //수식 나누기
    for(string s : expressions)
    {
        char ch[s.length()];
        strcpy(ch, s.c_str());
        char *ptr = strtok(ch, " =");
        while(ptr != NULL)
        {
            str[index].push_back(string(ptr));
            ptr = strtok(NULL, " =");
        }
        index++;
    }
    
    //가능한 진법의 최소 수 계산
    for(int i = 0; i < expressions.size(); i++)
    {
        for(int j = 0; j < str[i].size(); j++)
        {
            if(str[i][j] != "X" && str[i][j] != "+" && str[i][j] != "-")
            {
                for(int k = 0; k < str[i][j].length(); k++)
                    if(maxNum < str[i][j][k])
                        maxNum = str[i][j][k];
            }
        }
    }
    maxNum++;
    cout << maxNum << endl;
    
    int cnt = 0;
    
    //완성된 수식들을 확인하여 가능한 진법 찾기
    for(int binary = ((int)maxNum - 48); binary <= 9; binary++)
    {
        for(int i = 0; i < expressions.size(); i++)
        {
            cnt++;
            if(str[i][3] == "X")
                continue;
            
            if(str[i][1] == "+")
            {
                if(ChangeBinaryToNumber(binary, str[i][0]) + ChangeBinaryToNumber(binary, str[i][2]) != ChangeBinaryToNumber(binary, str[i][3]))
                {
                    cnt--;
                    break;
                }
            }
            else
            {
                if(ChangeBinaryToNumber(binary, str[i][0]) - ChangeBinaryToNumber(binary, str[i][2]) != ChangeBinaryToNumber(binary, str[i][3]))
                {
                    cnt--;
                    break;
                }
            }
        }
        
        if(cnt == expressions.size())
        {
            binaryNumbers.push_back(binary);
        }
        cnt = 0;
    }
    
    //X값 계산 후 저장
    unordered_map<string, int> m;
    unordered_map<string, int>::iterator iter;
    string s;
    
    for(int i = 0; i < expressions.size(); i++)
    {
        if(str[i][3] != "X")
            continue;
        for(int j = 0; j < binaryNumbers.size(); j++)
        {
            int binary = binaryNumbers[j];
            int val;
            if(str[i][1] == "+")
            {
                val = ChangeBinaryToNumber(binary, str[i][0]) + ChangeBinaryToNumber(binary, str[i][2]);
                s = ChangeNumberToBinary(binary, val);
            }
            else
            {
                val = ChangeBinaryToNumber(binary, str[i][0]) - ChangeBinaryToNumber(binary, str[i][2]);
                s = ChangeNumberToBinary(binary, val);
            }
            if(m.find(s) == m.end())
            {
                m.insert({s, binary});
            }
        }
        if(m.size() == 1)
        {
            iter = m.begin();
            s = str[i][0] + " " + str[i][1] + " " + str[i][2] + " = " + iter->first;
            answer.push_back(s);
        }
        else
        {
            s = str[i][0] + " " + str[i][1] + " " + str[i][2] + " = ?";
            answer.push_back(s);
        }
        m.clear();
    }
    
    return answer;
}
```