## 문제 요약

입국 심사마다 걸리는 시간 다름.

심사대마다 한명씩 심사 가능, 심사관마다 걸리는 시간이 있음.

모든 사람이 심사를 받는데 걸리는 최소시간이 되도록하기

## 핵심 조건 및 제약 사항

입국심사 기다리는 사람 → 1 ~ 1,000,000,000

심사 시간 → 1 ~ 1,000,000,000

심사관 → 1 → 100,000

##  시행착오

완전탐색하기엔 범위가 너무 큼.

A 시간에 모든 사람을 심사할 수 있을까? → 결정 알고리즘 → 좁혀나가며 최소 찾기 → 이분 탐색

## 구현

```java
import java.util.*;

class Solution {
    public long solution(int n, int[] times) {
        long answer = 0;
        
        Arrays.sort(times);
        
        long lt = 1;
        long rt = (long)times[0] * n;
        
        while(lt <= rt){
            long mid = (lt + rt) / 2;
            
            long cnt = 0;
            for(int i=0; i<times.length; i++){
                cnt += mid / times[i];
            }
            
            //모두 심사 가능하면 갱신하고 더 좁혀보기
            if(cnt >= n){
                answer = mid;
                rt = mid-1;
            }
            
            //불가능하면 좀 더 늘려서 다시 보기
            else{
                lt = mid+1;
            }
        }

        return answer;
    }
}
```

## 회고

오버플로우 방지 까먹지 말기! 연산 시 미리 캐스팅하기!