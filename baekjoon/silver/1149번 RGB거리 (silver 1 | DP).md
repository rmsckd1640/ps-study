## 문제 요약

모든 집에 색을 칠하는 데, 비용이 최소가 되도록.

이웃한 집에는 서로 색이 달라야 함.

## 핵심 조건 및 제약 사항

집의 수 N → 1 ~ 1000

비용 1 ~ 1000

## 시행착오

현재 집(i)을 특정 색으로 칠할 때의 최소 비용은, 이전 집(i-1)에서 다른 색깔 두 가지 중 더 저렴한 경로를 선택해온 결과.

`dp[i][color]` → i번째 집을 `color`로  칠했을 때의 누적 최소 비용으로 정의.

## 구현

시간복잡도: 집의 개수만큼 반복문을 한 번 돌기 때문에 $O(N)$

```java
import java.util.*;

public class Main{
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int[][] arr = new int[n][3];
		for(int i=0; i<n; i++) {
			for(int j=0; j<3; j++) {
				arr[i][j] = sc.nextInt();
			}
		}
		
		int[][] dp = new int[n][3];
		
		dp[0][0] = arr[0][0];
		dp[0][1] = arr[0][1];
		dp[0][2] = arr[0][2];
		
		for(int i=1; i<n ; i++) {
			dp[i][0] = arr[i][0] + Math.min(dp[i-1][1], dp[i-1][2]);
			dp[i][1] = arr[i][1] + Math.min(dp[i-1][0], dp[i-1][2]);
			dp[i][2] = arr[i][2] + Math.min(dp[i-1][1], dp[i-1][0]);
		}
		
		System.out.print(Math.min(dp[n-1][0], Math.min(dp[n-1][1], dp[n-1][2])));
	}
}
```