### 출처 - https://school.programmers.co.kr/learn/courses/30/lessons/92334
## 사용언어 - java
## 해설 - https://velog.io/@estelle17/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%AC%B8%EC%A0%9C-%EC%8B%A0%EA%B3%A0-%EA%B2%B0%EA%B3%BC-%EB%B0%9B%EA%B8%B0

```java
import java.util.*;

class Solution {
    
    public int[] solution(String[] id_list, String[] report, int k) {
        int[] answer = new int[id_list.length];
        int[][] userMap = new int[id_list.length+1][id_list.length];
        HashMap<String, Integer> map = new HashMap<String, Integer>();
        
        //String으로 된 이름을 Integer로 쉽게 접근하기 위해 해시맵에 (이름, 번호)로 저장
        for(int i = 0; i < id_list.length; i++)
        {
            map.put(id_list[i], i);
        }
        //2차원 배열에 신고한 이력 저장
        for(int i = 0; i < report.length; i++)
        {
            String[] split = report[i].split(" ");
            if(userMap[map.get(split[0])][map.get(split[1])] == 0)
            {
                userMap[map.get(split[0])][map.get(split[1])]++;
                userMap[id_list.length][map.get(split[1])]++;
            }
        }
        //신고한 사람들 중 k이상으로 신고된 사람은 메일을 받은 횟수를 더해줌
        for(int i = 0; i < id_list.length; i++)
        {
            if(userMap[id_list.length][i] >= k)
            {
                for(int j = 0; j < id_list.length; j++)
                {
                    if(userMap[j][i] == 1)
                        answer[j]++;
                }
            }
        }
        
        return answer;
    }
}
```