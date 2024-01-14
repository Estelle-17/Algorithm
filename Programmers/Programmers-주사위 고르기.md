### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/258709
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%A3%BC%EC%82%AC%EC%9C%84-%EA%B3%A0%EB%A5%B4%EA%B8%B0

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<vector<int>> dices;
int n;

vector<int> dfsArray;
map<vector<int>, vector<int>>::iterator iter;

void dfs(vector<int> a, int index, int sum) //각 주사위를 굴린 모든 경우 계산
{
    if(a.size() == index){
        dfsArray.push_back(sum);
        return;
    }
    
    for(int i = 0; i < 6; i++)
    {
        sum += dices[a[index]-1][i];
        dfs(a, index+1, sum);
        sum -= dices[a[index]-1][i];
    }
}

vector<int> checkWin(vector<int> a, vector<int> b)  //A와 B가 굴린 주사위들의 합들을 가지고 승패 계산
{
    sort(a.begin(), a.end());
    sort(b.begin(), b.end());
    vector<int> v(3, 0);
    
    int AIndex = 0;
    int BIndex = 0;
    int win = 0;
    int draw = 0;
    while(AIndex < a.size())
    {
        if(a[AIndex] > b[b.size()-1])   //A주사위값이 B주사위 중 제일 큰 값보다 더 크다면 
        {
            v[0] += b.size();
            AIndex++;
        }
        else if(a[AIndex] < b[BIndex])  //A가 더 클 경우
        {
            v[0] += win;
            AIndex++;
        }else if(a[AIndex] == b[BIndex])    //같으면 무승부로 처리
        {
            int t = b[BIndex];
            while(t == b[BIndex])
            {
                draw++;
                BIndex++;
            }
            t = a[AIndex];
            while(t == a[AIndex])
            {
                v[0] += win;
                v[1] += draw;
                AIndex++;
            }
            win += draw;
            draw = 0;
        }else   //B가 더 클 경우
        {
            int t = b[BIndex];
            while(t == b[BIndex])
            {
                win++;
                BIndex++;
            }
        }
    }
    
    return v;
}

vector<int> solve() //주어진 주사위들로 순열을 만들어줌
{
    //주사위에 번호 생성
    vector<int> vec(dices.size(), 0);
    for(int i = 0; i < dices.size(); i++)
    {
        vec[i] = i+1;
    }
    //순열을 위한 배열
    vector<int> ADice(dices.size(), 1);
    vector<int> BDice(dices.size() - n, 1);
    vector<int> AComb;
    vector<int> BComb;
    
    pair<int, int> maxWin = make_pair(0, 0);
    vector<int> result;
    
    for(int i = 0; i < n; i++)  //A와 B가 가질 주사위의 수 n/2 만큼 0으로 변환
    {
        ADice[i] = 0;
        BDice[i] = 0;
    }
    do{ //next_permutation과 0,1로 이루어진 배열을 가지고 중첩되지 않는 순열 생성
        vector<int> v1;
        vector<int> v2;
        vector<int> v3;
        for(int i = 0; i < ADice.size(); i++)
        {
            if(ADice[i] == 0)   //A의 주사위
                v1.push_back(vec[i]);
            else    //B의 주사위
                v2.push_back(vec[i]);
        }
        if(v2.size() > n)   //만약 주어진 주사위가 홀수라면
        {
            do{ //B의 주사위 중 n/2만큼만 선택
                for(int j = 0; j < BDice.size(); j++)
                {
                    if(BDice[j] == 0)
                    {
                        v3.push_back(v2[j]);
                    }
                }
                //A와 B의 각 주사위를 굴린 모든 경우 계산
                dfsArray = vector<int>();
                dfs(v1, 0, 0);
                AComb = dfsArray;
                dfsArray = vector<int>();
                dfs(v3, 0, 0);
                BComb = dfsArray;
                //승리, 무승부 횟수 게산
                vector<int> check = checkWin(AComb, BComb);
                if(maxWin.first < check[0] || maxWin.first == check[0] && maxWin.second < check[1])
                {
                    maxWin = make_pair(check[0], check[1]);
                    result = v1;
                }
            } while(next_permutation(BDice.begin(), BDice.end()));
        }else{
            //A와 B의 각 주사위를 굴린 모든 경우 계산
            dfsArray = vector<int>();
            dfs(v1, 0, 0);
            AComb = dfsArray;
            dfsArray = vector<int>();
            dfs(v2, 0, 0);
            BComb = dfsArray;
            //승리, 무승부 횟수 게산
            vector<int> check = checkWin(AComb, BComb);
            if(maxWin.first < check[0] || maxWin.first == check[0] && maxWin.second < check[1])
            {
                maxWin = make_pair(check[0], check[1]);
                result = v1;
            }
        }
    } while(next_permutation(ADice.begin(), ADice.end()));
    
    //제일 승리의 수가 크고, 그다음으로 무승부의 수가 큰 수를 반환
    return result;
}

vector<int> solution(vector<vector<int>> dice) {
    vector<int> answer;
    dices = dice;
    n = dice.size() / 2;    //주사위를 가져야하는 수
    
    answer = solve();
    
    return answer;
}
```