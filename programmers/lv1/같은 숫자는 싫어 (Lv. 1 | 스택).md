## 문제 요약

배열에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거 후, 제거된 남은 수들을 반환하는데 순서를 유지해야함.

## 핵심 조건 및 제약 사항

배열의 크기 ~ 1,000,000
배열 원소의 크기 → 0 ~ 9

## 시행착오

연속된 숫자인지 확인하며 비교하기 위해 스택을 사용한다.

stack의 top을 확인하여 본인과 같으면 스택에 넣지 않는다.

최종적으로 pop해서 배열의 맨뒷자리부터 넣는다.

## 구현

```java
import java.util.*;

public class Solution {
    public int[] solution(int []arr) {
        Deque<Integer> stack = new ArrayDeque<>();
        
        for(int i=0; i<arr.length; i++){
            //스택이 비어있지 않는데, 스택의 탑과 같다면 다음으로 넘어가기.
            if(!stack.isEmpty() && stack.peek() == arr[i]){
                continue;
            }
            
            stack.push(arr[i]);
        }
        
        int[] answer = new int[stack.size()];
        int size = stack.size();
        
        for(int i=size-1; i >= 0 ; i--){
            answer[i] = stack.pop();
        }
        
        return answer;
    }
}
```