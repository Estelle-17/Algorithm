### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42884
## 사용언어 - C++

```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

bool compare(vector<int> a, vector<int> b)  { return a[1] < b[1]; }

int solution(vector<vector<int>> routes) {
    int answer = 0;
    vector<int> camera;
    int cameraPoint;
    bool isCameraInstall;
    
    //도착지점을 기준으로 오름차순해줍니다
    sort(routes.begin(), routes.end(), compare);
        
    for(int i = 0; i < routes.size(); i++)
    {
        //설치된 카메라들에 차량이 지나가는지 확인합니다.
        for(int temp : camera)
        {
            //카메라가 설치된 지점에 차량이 지나간다면 설치하지 않고 넘어갑니다
            if(temp >= routes[i][0] && temp <= routes[i][1] && !isCameraInstall)
            {
                isCameraInstall = true;
                break;
            }
        }
        //설치된 카메라에 차량이 지나가지 않는다면 도착 지점에 카메라를 설치합니다
        if(!isCameraInstall)
        {
            camera.push_back(routes[i][1]);
            answer++;
        }
        isCameraInstall = false;
    }
    
    return answer;
}
```