### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/49995
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%BF%A0%ED%82%A4-%EA%B5%AC%EC%9E%85

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> cookie) {
    int answer = 0;
    int maxSum = 0;
    
    for(int i = 0; i < cookie.size()-1; i++)
    {
        //한 인덱스를 기준으로 두 아들한테 바구니의 과자만큼 더해줌
        int leftIndex = i;
        int rightIndex = i+1;
        int leftSum = cookie[leftIndex];;
        int rightSum = cookie[rightIndex];
        while(true)
        {
            if(leftSum > rightSum)  //첫 번째 아들이 더 작을 경우
            {
                rightIndex++;
                if(rightIndex >= cookie.size())
                {
                    break;
                }
                rightSum += cookie[rightIndex];
            }
            else if(leftSum < rightSum)  //두 번째 아들이 더 작을 경우
            {
                leftIndex--;
                if(leftIndex < 0)
                {
                    break;
                }
                leftSum += cookie[leftIndex];
            }
            else    //과자의 수가 같을 경우
            {
                maxSum = max(maxSum, leftSum);
                leftIndex--;
                if(leftIndex < 0)
                {
                    break;
                }
                leftSum += cookie[leftIndex];
            }
        }
        answer = max(answer, maxSum);
    }
    
    return answer;
}
```