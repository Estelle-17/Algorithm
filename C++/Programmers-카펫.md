### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42842
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%B9%B4%ED%8E%AB

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int brown, int yellow) {
    vector<int> answer;
    
    int height = 3;
    int width = (brown + yellow) / height;

    //노란색과 갈색을 더한 크기와 같으며, 노란색 격자 수만큼 직사각형이 가능한 카펫 크기 체크
    while(width >= height)
    {
        if(width * height != brown + yellow)
        {
            height++;
            width = (brown + yellow) / height;
            continue;
        }
        int w, h;
        w = width-1;
        h = height-1;
        
        while(w > 0 || h > 0)
        {
            if(w * h == yellow)
            {
                answer.push_back(width);
                answer.push_back(height);
                break;
            }
            w--;
            h--;
        }
        if(answer.size() != 0)
        {
            break;
        }
        height++;
        width = (brown + yellow) / height;
    }
    
    return answer;
}
```