### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/258707
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-GPS

-푸는중-
```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int n;
int coinCount;
queue<int> player;
vector<int> checkNum;
queue<int> nextCard;
pair<int, int> p;

bool checkCard(int checkNumber)
{
    int qSize = player.size();
    //cout << "checkNumber : " << checkNumber << endl;
    for(int i = 0; i < qSize; i++)  //현재 카드로 n+1수를 만들 수 있는지 확인
    {
        int num = player.front();   player.pop();
        if(checkNum[num] == 0)
            continue;
        
        //cout << num << ":" << checkNum[num] << ", " << (n+1)-num << ":" << checkNum[(n+1)-num] << " -> " << n+1 << endl;
        if(checkNum[(n+1)-num] <= checkNumber && checkNum[(n+1)-num] != 0){
            if(checkNumber == 2 && coinCount < checkNum[(n+1)-num]-1 + checkNum[num]-1) //n+1수를 만드는데 뽑은 카드를 사용했지만 코인이 부족했을 경우
                continue;
            
            
            coinCount -= (checkNum[(n+1)-num]-1) + (checkNum[num]-1);
            //cout << num << ", " << (n+1)-num << " coin : " << coinCount << endl;
            p = make_pair(checkNum[num], checkNum[(n+1)-num]);
            checkNum[num] = 0;
            checkNum[(n+1)-num] = 0;
            return true;
        }
        player.push(num);
    }
    
    return false;
}

int solution(int coin, vector<int> cards) {
    int answer = 0;
    n = cards.size();
    checkNum.assign(1001, 0);
    int index = n/3;
    coinCount = coin;
    
    for(int i = 0; i < n/3; i++)
    {
        player.push(cards[i]);
        checkNum[cards[i]] = 1;
    }
    
    while(true)
    {
        answer++;
        if(index >= cards.size()-1)
            break;
        
        bool isComplete = false;
        //nextCard.push(cards[index]);
        //nextCard.push(cards[index+1]);
        checkNum[cards[index]] = 2;
        checkNum[cards[index+1]] = 2;
        player.push(cards[index]);
        player.push(cards[index+1]);
        index += 2;
        
        isComplete = checkCard(1);
        if(isComplete) //현재 있는 카드로 체크
            continue;
        
        if(coinCount > 0)
        {
            isComplete = checkCard(2);
            if(isComplete)
                continue;
        }
        if(!isComplete)
            break;
        /*if(coin > 0 && nextCard.size() >= 1)
        {
            for(int i = 0; i < nextCard.size(); i++)
            {
                queue<int> temp = player;
                int num = nextCard.front(); nextCard.pop();
                checkNum[num] = 1;
                player.push(num);
                if(checkCard(num, -1)) { //추가 후 있는 카드로 체크
                    cout << "add : " << num << " - coin : " << coin << endl;
                    isComplete = true;
                    coin--;
                    break;
                }
                nextCard.push(num);
                checkNum[num] = 0;
                player = temp;
            }
            if(!isComplete && nextCard.size() >= 2)
            {
                vector<int> vec(nextCard.size(), 1);
                vec[0] = 0;
                vec[1] = 0;
                
                do{
                    queue<int> temp = player;
                    int num = nextCard.front(); nextCard.pop();
                    checkNum[num] = 1;
                    player.push(num);
                    if(checkCard(num, -1)) { //추가 후 있는 카드로 체크
                        cout << "add : " << num << " - coin : " << coin << endl;
                        isComplete = true;
                        coin--;
                        break;
                    }
                    nextCard.push(num);
                    checkNum[num] = 0;
                    player = temp;
                }while(next_permutation(vec.begin(), vec.end()))
            }
            
        }
        if(!isComplete)
            break;*/
    }
    
    return answer;
}
```