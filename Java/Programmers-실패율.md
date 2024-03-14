### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/42889
## 사용언어 - java
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%8B%A4%ED%8C%A8%EC%9C%A8

```java
import java.util.*;

class Solution {
    public int[] solution(int N, int[] stages) {
        int[] answer = new int[N];
        float[] stageSuccessRate = new float[N+2];
        int[] stageSuccessCount = new int[N+2];
        Integer[] result = new Integer[N];
        int playerNum;
        //스테이지 승리 횟수 저장
        for(int i = 0; i < stages.length; i++)
        {
            stageSuccessCount[stages[i]]++;
        }
        playerNum = stages.length;  //전체 플레이어 수
        //스테이지의 실패율 계산
        for(int i = 1; i < N+1; i++)
        {
            result[i-1] = i;
            if(stageSuccessCount[i] > 0)
            {
                stageSuccessRate[i] = (float)stageSuccessCount[i] / (float)playerNum;
                playerNum -= stageSuccessCount[i];
            }
        }
        //실패율을 기준으로 내림차순 정렬
        Arrays.sort(result, new Comparator<Integer>() {
            @Override
            public int compare(Integer i1, Integer i2) {
                if(stageSuccessRate[i2] - stageSuccessRate[i1] == 0 && i2 > i1) //승률이 낮으면 스테이지가 적은 곳으로
                    return -1;
                if(stageSuccessRate[i2] - stageSuccessRate[i1] < 0)
                    return -1;
                else
                    return 1;
            }
        });
        for(int i = 0; i < N; i++)
        {
            answer[i] = result[i];
        }
        
        return answer;
    }
}
```