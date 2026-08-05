## 문제 요약

참가한 마라톤 선수들 중에 단 한명의 선수를 제외하고 모두 완주함.

완주하지 못한 선수의 이름을 return

## 핵심 조건 및 제약 사항

참가한 선수의 수 → 1 ~ 100,000

참가자 이름 배열의 길이는 완주한 선수 이름 배열보다 1 작다.

참가자 이름 길이 → 1 ~ 20 & 알파벳 소문자

참가자 중에 동명이인 있을 수 있음.

## 시행착오

참가자마다 완주자 목록을 탐색하는 방식은 O($N^2$)이다. N이 최대 100,000이기 때문에 시간 초과가 발생할 수 있다.

이를 개선하기 위해 Map 자료구조를 사용하여 이름별 등장 횟수를 관리했다.

참가자를 순회하며 등장 횟수를 저장하고, 완주자를 순회하며 해당 이름의 횟수를 감소시키면 된다.

마지막으로 value가 0이 아닌 key가 완주하지 못한 참가자이다.

이 경우 시간복잡도는 O($N$)이다.

## 구현

```java
import java.util.*;

class Solution {
    public String solution(String[] participant, String[] completion) {

        Map<String, Integer> map = new HashMap<>();

        for(int i = 0; i < participant.length; i++){
            map.put(participant[i], map.getOrDefault(participant[i], 0) + 1);
        }

        for(int i = 0; i < completion.length; i++){
            map.put(completion[i], map.get(completion[i]) - 1);
        }

        for(Map.Entry<String, Integer> entry : map.entrySet()){
            if(entry.getValue() != 0) {
                return entry.getKey();
            }
        }

        return "";
    }
}
```