## 문제 요약

폰켓몬 수의 절반만 가져갈 수 있는데, 최대한 여러 종류를 골라야 함.

## 핵심 조건 및 제약 사항

- 주어지는 폰케못 수 짝수
- nums 길이 1 ~ 10000
- 폰켓몬 종류 번호 1 ~ 200,000

## 시행착오

각 종류의 수를 파악하기 위해 `HashMap`이용.

n마리를 가져가야하는데 종류 수가 n마리 보다 작다면 최대로 고를 수 있는 종류 수는 그대로가 답.

하지만 종류 수가 n보다 크거나 같다면 최대로 고를 수 있는 종류 수는 n마리가 답. 뭘 고르든 그것이 최대.

## 구현

```java
import java.util.*;

class Solution {
    public int solution(int[] nums) {
        int answer = 0;
        
        HashMap<Integer, Integer> map = new HashMap<>();
        
        for(int i=0; i<nums.length; i++){
            map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);
        }
        
        int size = map.size(); //종류수
        int sel = nums.length / 2; //골라야 하는 수
        
        if(size <= sel) answer = size;
        else answer = sel;
        
        return answer;
    }
}
```