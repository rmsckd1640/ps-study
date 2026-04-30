[정올 1141번 불쾌한 날](https://jungol.co.kr/problem/1141)

## 문제 요약

소들이 오른쪽을 바라보고 줄서있는데, 자신의 키보다 작은 소들만 볼 수 있다.

각 소들이 볼 수 있는 수를 다 더한 값을 출력해라.

## 핵심 조건 및 제약 사항

소 마릿수 1 ~ 80000

소 키 1 ~ 1,000,000,000

## 시행착오

자기보다 크거나 같은 소가 나오기 전까지 스택에 쌓아야 겠다는 생각을 했다.

스택의 탑보다 다음 소가 작으면 넣고, 크거나 같다면 그 사이에 소들은 볼 수 있는 것이기 때문에 서로의 인덱스 차이에 -1을 하면 된다. -1 하는 이유는 큰 소는 못보기 때문이다.

스택에 남아있는 경우 바닥기준 내림차순으로 들어가 있으니 맨 마지막과 나머지 애들의 인덱스 차이를 계산하면된다. 여기서는 볼 수 있는 소가 맨마지막도 포함이니 -1을  안해도 된다.

## 구현

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int[] arr = new int[n];

        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
        }

        Deque<Integer> stack = new ArrayDeque<>();
        long answer = 0;

        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && arr[stack.peek()] <= arr[i]) {
                int temp = stack.pop();
                answer += i - temp - 1;
            }
            stack.push(i);
        }

        // 나머지 처리
        int temp = stack.pop();
        while (!stack.isEmpty()) {
            answer += temp - stack.pop();
        }

        System.out.println(answer);
    }
}
```

## 회고

제출했는데 90.9점이 나왔다. 이유를 찾기 힘들었는데 알고보니 `answer`의 범위 문제였다.

총 볼 수 있는 소의 마릿수가 int범위를 벗어날 수 있다는 점을 생각치 못했다.

만약 소가 큰 소부터 작아지는 순으로 80000마리가 쭉 서있다면 볼 수 있는 수의 합이 79999 + 79998 + ... 1 까지 더하게 되는데 이때 계산을 해보면 `(79999 * 79998) / 2` → 30억이 넘는 즉, int의 범위인 21억을 넘게 된다. 그래서 `answer`를 `long`으로 선언했어야 했다.

다음부턴 시간복잡도 뿐만이 아니라 변수를 선언할 때 어떤 변수형을 써야할지도 잘 체크해봐야겠다.

## 추가 구현

지금은 "자기가 바라볼 수 있는 소가 몇마리인가?" 기준으로 풀었지만 반대로 "나를 바라볼 수 있는 소는 몇마리인가?"로도 풀 수 있다.

쉽게 설명하자면, 어떤 소의 차례가 왔을 때

1. 첫번째 풀이 → "나보다 큰 놈이 오기전까지 스택에 있어야지"
2. 두번째 풀이 → "스택에 있는 애들은 다 날 볼 수 있어. 나보다 작은 애는 스택에 있으면 안돼"

```java
import java.util.*;

public class Main{
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		int n = sc.nextInt();
		
		int[] arr = new int[n];
		
		for(int i=0; i<n; i++) {
			arr[i] = sc.nextInt();
		}
		
		Deque<Integer> stack = new ArrayDeque<>();
		long answer = 0;
		for(int i=0; i<n; i++) {
			// 스택에서 날 볼 수 없는 애들 없애
			while(!stack.isEmpty() && arr[stack.peek()] <= arr[i]) { 
				stack.pop();
			}
			// 스택에 있는 애들은 날 다 볼 수 있어. 다 더해.
			answer += stack.size(); 
			// 이제 나도 구경꾼
			stack.push(i); 
		}
		
		System.out.println(answer);
	}
}
```