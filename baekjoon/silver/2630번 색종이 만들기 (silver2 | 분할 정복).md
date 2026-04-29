[백준 2630번 색종이 만들기](https://www.acmicpc.net/problem/2630)

## 문제 요약

전체 종이가 모두 같은 색으로 칠해져 있지 않으면 똑같은 크기 4개로 4등분 함. 같은 색으로 칠해져 있으면 하나의 종이. → 반복

하얀색 색종이의 개수와 파란색 색종이의 개수 출력.

## 핵심 조건 및 제약 사항

0은 하얀색, 1은 파란색

N은 2, 4, 8, 16, 32, 64, 128 중 하나.

## 시행착오

색을 확인하다 다르면 4등분하는 분할 정복 문제.

등분을 해서 새로운 종이로 들어갈 때 그 종이의 크기와 시작점을 파라미터로 전달.

## 구현

각 단계마다 n → n/2 → n/4 → ... → 1
→ 총 $logn$ 그리고 각 단계에서 모든 색깔 확인하는 $n^2$
따라서 총 시간복잡도 → $O(n^2logn)$

```java
import java.util.*;

public class Main{
	static int[][] arr;
	static int zero;
	static int one;
	public static void DFS(int n, int r, int c) {
		if(n == 1) {
			if(arr[r][c] == 1) one++;
			else zero++;
			return;
		}
		
		int cur = arr[r][c];
		boolean flag = false;
		
		for(int i=r; i<r+n; i++) {
			for(int j=c; j<c+n; j++) {
				if(arr[i][j] != cur) {
					flag = true;
					break;
				}
			}
			if(flag) break;
		}
		
		if(!flag) {
			if(cur == 0) zero++;
			else one++;
		}
		
		else {
			int size = n/2;
			
			//왼쪽 위
			DFS(size, r, c);
			//오른쪽 위
			DFS(size, r, c+size);
			//왼쪽 아래
			DFS(size, r+size, c);
			//오른쪽 아래
			DFS(size, r+size, c+size);
		}
	}
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		arr = new int[n][n];
		for(int i=0; i<n; i++) {
			for(int j=0; j<n; j++) {
				arr[i][j] = sc.nextInt();
			}
		}
		DFS(n, 0, 0);
		
		System.out.println(zero + "\n" + one);
	}

}
```

## 회고

처음에 탐색할 때 (0, 0)기준으로 했다가 이렇게 되면 만약에 3사분면을 갔는데 (0,0)부터 확인하는 이상한 현상이 발생하는 것을 깨닫고 다음 종이의 시작점을 (r,c)로 잘 전달해줬다. → 전체 기준이 아닌 부분(현재) 문제를 기준으로 생각해야한다!