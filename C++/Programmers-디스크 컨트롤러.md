### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42627
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EB%94%94%EC%8A%A4%ED%81%AC-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC
```cpp
#include <bits/stdc++.h>
#include <string>
#include <vector>

using namespace std;

struct process{
    int processTime;
    int startTime;
    int index;
    process(int a, int b, int c) : processTime(a), startTime(b), index(c)   {}
};

struct compare {
    bool operator()(process a, process b) {
        if(a.processTime > b.processTime)
        {
            return true;
        }else{
            if(a.processTime == b.processTime)
            {
                return a.startTime > b.startTime;
            }
            return false;
        }
    }
};

int solution(vector<vector<int>> jobs) {
    int answer = 0;
    int time = 0;
    priority_queue<process, vector<process>, compare> pq;
    
    sort(jobs.begin(), jobs.end());
    
    answer = jobs[0][1];
    time = jobs[0][0] + jobs[0][1];
    
    int n = 1;
    while(n < jobs.size())
    {
        if(time >= jobs[n][0])
        {
            pq.push(process(jobs[n][1], jobs[n][0], n));
            n++;
        }
        else
        {
            if(pq.empty())
            {
                //바로 다음 처리 시간만큼 진행
                answer += jobs[n][1];
                time += (jobs[n][0] - time) + jobs[n][1];
                n++;
            }else
            {
                //대기가 짧은 순서대로, 같으면 처리시간이 적은 순으로 계산
                process cur = pq.top();
                pq.pop();
                answer += (time - jobs[cur.index][0]) + jobs[cur.index][1];
                time += jobs[cur.index][1];
            }
        }
    }
    while(!pq.empty())
    {
        //대기가 짧은 순서대로, 같으면 처리시간이 적은 순으로 계산
        process cur = pq.top();
        pq.pop();
        answer += (time - jobs[cur.index][0]) + jobs[cur.index][1];
        time += jobs[cur.index][1];
    }
    
    answer = answer / jobs.size();
    
    return answer;
}
```