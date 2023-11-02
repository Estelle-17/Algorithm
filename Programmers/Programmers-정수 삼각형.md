### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/43105
## 사용언어 - C++

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

int solution(vector<vector<int>> triangle) {
    int answer = 0;
    
    for(int i = 1; i < triangle.size(); i++)
    {
        for(int j = 0; j < i + 1; j++)
        {
            //맨 왼쪽과 오른쪽의 수는 바로 더해주기
            //그외의 수는 위의 두 값을 비교하여 더 큰 수를 입력함
            if(j == 0)
            {
                triangle[i][j] += triangle[i-1][j];
            }
            else if(j == i)
            {
                triangle[i][j] += triangle[i-1][j-1];
            }
            else
            {
                int left = triangle[i][j] + triangle[i-1][j-1];
                int right = triangle[i][j] + triangle[i-1][j];
                left > right ? triangle[i][j] = left : triangle[i][j] = right;
            }
        }
    }
    
    //맨 아래의 제일 큰 수를 정렬하여 찾아냄
    sort(triangle[triangle.size()-1].begin(), triangle[triangle.size()-1].end());
    answer = triangle[triangle.size()-1][triangle.size()-1];

    return answer;
}
```