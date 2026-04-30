## 문제 요약

2가지 종류의 타일이 있다.

타일 회전 가능.

2가지 타일들을 여러개 사용해서 가로 2칸, 세로 N칸 채우는 방법의 수는?

## 핵심 조건 및 제약 사항

N → 1 ~ 100,000

##  시행착오

세로가 1칸일 때 → 1가지

세로가 2칸일 때 → 3가지

세로가 3칸일 때 → 2칸이였을 때 위에다 한 칸짜리 올리기(i-1) + 1칸 이였을 때 위에 두 칸짜리 올리는데 두 칸의 경우는 2가지여서(i-2)·2

점화식 → $dp[i] = (dp[i-1] + dp[i-2] * 2)$

## 구현

```java
import java.util.*;

public class Main{
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		
		int[] dp = new int[n+1];
		
		dp[1] = 1;
		
		if(n >= 2) dp[2] = 3;
		
		for(int i=3; i<=n; i++) {
			dp[i] = (dp[i-1] + dp[i-2] * 2) % 20100529;
		}
		
		System.out.print(dp[n]);
	}
}
```