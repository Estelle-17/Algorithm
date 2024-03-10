### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/150365
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%AF%B8%EB%A1%9C-%ED%83%88%EC%B6%9C-%EB%AA%85%EB%A0%B9%EC%96%B4

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

//d >> l >> r >> u 사전순
string solution(int n, int m, int x, int y, int r, int c, int k) {
    string answer = "";
    int dist = abs(x-r) + abs(y-c);
    //만약 도착이 불가능한 k가 주어진다면 return
    if(k-dist < 0 || (k-dist)%2 != 0)
        return "impossible";
    
    for(int i = 0; i < k; i++)
    {
        dist = abs(x-r) + abs(y-c);
        if(dist < k-i) //남은 이동 횟수가 도착지점까지 가는 이동 횟수보다 더 클 경우
        {
            //맨 아래 -> 맨 왼쪽 -> 오른쪽 순으로 움직임
            if(x < n)
            {
                answer = answer + "d";
                x++;
            }else if(y > 1)
            {
                answer = answer + "l";
                y--;
            }else
            {
                answer = answer + "r";
                y++;
            }
        }else
        {
            //아래 -> 왼쪽 -> 오른쪽 -> 위 순서로 도착지점으로 이동
            if(x < r)
            {
                answer = answer + "d";
                x++;
            }else if(y != c)
            {
                if(y < c)
                {
                    answer = answer + "r";
                    y++;
                }else
                {
                    answer = answer + "l";
                    y--;
                }
            }else
            {
                answer = answer + "u";
                x--;
            }
        }
    }
    
    //만약 도착지점이 도착하지 않았을 경우
    if(x != r || y != c)
    {
        answer = "impossible";
    }
    
    return answer;
}
```