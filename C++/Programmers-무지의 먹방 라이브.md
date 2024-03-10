### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42891
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%AC%B4%EC%A7%80%EC%9D%98-%EB%A8%B9%EB%B0%A9-%EB%9D%BC%EC%9D%B4%EB%B8%8C

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> food_times, long long k) {
    int answer = 0;
    vector<int> foods = food_times;
    int foodCount = food_times.size();
    
    //오름차순 정렬
    sort(foods.begin(), foods.end());
    
    long long sum = 0;
    long long prevfood = 0;
    int index = 0;
    while(sum+foodCount*(foods[index] - prevfood) <= k)
    {
        if(foods[index] - prevfood > 0)
        {
            sum += foodCount*(foods[index] - prevfood);
            prevfood = foods[index];
        }
        foodCount--;
        index++;
        if(index == foods.size())
        {
            return -1;
        }
    }
    
    int cur = foods[index];
    while(k-sum >= foodCount)
    {
        sum += foodCount;
    }
    index = 0;
    while(sum != k+1)
    {
        if(food_times[index] - cur >= 0)
        {
            sum++;
        }
        index++;
    }
    answer = index;
    
    return answer;
}
```