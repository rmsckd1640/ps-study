[백준 2667번 단지번호붙이기](https://www.acmicpc.net/problem/2667)

## 문제 요약

정사각형 모양(NxN)의 지도에서 단지 수를 세고 각 단지에 속하는 집의 개수를 오름차순으로 정렬하여 출력.

## 핵심 조건 및 제약 사항

연결되었다는건 상하좌우. 대각선은 아님.

지도의 크기 N의 범위 → 5 <= N <= 25

집이 있으면 1, 없으면 0.

**각 단지의 집의 수를 오름차순으로 정렬하여 출력.**

## 시행착오

2차원 배열을 돌면서 1을 만났을 때 방문을 하지 않았다면 BFS 탐색 시작.

탐색 시작과 동시에 새로운 단지를 발견한 것이므로 카운트.

BFS 탐색을 하며 단지내에 아파트 수 세기.

상하좌우 확인을 위한 방향 배열 사용 및 좌표를 저장하는 객체 만들기.

단지 수가 몇개인지 모르기 때문에 아파트 수는 리스트에 담기.

2차원 배열 입력시 띄어쓰지 않아서 직접 나눠야함. → 문자열로 입력받고 각 줄마다 한 글자씩 숫자로 변환해서 넣었음.

연결된 곳을 재귀호출하여 파고드는 DFS 방법으로도 풀어봄.

DFS 구현 중 맨 위에 base case 고민하다 틀림 → 이 문제에서는 DFS 호출 전에 조건을 모두 검사해서 유효한 경우에만 재귀를 호출하므로, 별도의 base case가 필요없다.

## 구현

- BFS: 큐에서 꺼낼 때 방문 처리 및 카운트
- DFS: 함수 진입 시 방문 처리 및 카운트

### 1. BFS

```java
import java.util.*;

class Point{
	int x;
	int y;
	
	public Point(int x, int y) {
		this.x = x;
		this.y = y;
	}
}

public class Main {
	static List<Integer> list = new ArrayList<>();
	static int[] dx = {-1, 1, 0, 0};
	static int[] dy = {0, 0, -1, 1};
	static int cnt = 0;
	
	public static void BFS(int x, int y, int[][] arr, int[][] ch, int n) {
		cnt = 0;
		
		Deque<Point> q = new ArrayDeque<>();
		
		ch[x][y] = 1;
		q.offer(new Point(x, y));
		
		while(!q.isEmpty()) {
			Point cur = q.poll();
			cnt++;
			
			for(int i=0; i<4; i++) {
				int nx = cur.x + dx[i];
				int ny = cur.y + dy[i];
				
				if(nx >= 0 && nx < n && ny >= 0 && ny < n && ch[nx][ny] == 0 && arr[nx][ny] == 1) {
					ch[nx][ny] = 1;
					q.offer(new Point(nx, ny));
				}
			}
		}
	}
	
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[][] arr = new int[n][n];
        int[][] ch = new int[n][n];
        String[] temp = new String[n];
        for(int i=0; i<n; i++) {
        	temp[i] = sc.next();
        }
        for(int i=0; i<n; i++) {
        	String str = temp[i];
        	for(int j=0; j<n; j++) {
        		arr[i][j] = str.charAt(j) - '0';
        	}
        }
        
        int total = 0;
        List<Integer> list = new ArrayList<>();
        for(int i=0; i<n; i++) {
        	for(int j=0; j<n; j++) {
        		if(ch[i][j] == 0 && arr[i][j] == 1) {
        			total++;
        			BFS(i, j, arr, ch, n);
        			list.add(cnt);
        		}
        	}
        }
        
        Collections.sort(list);
        
        System.out.println(total);
        for(int i : list) {
        	System.out.println(i);
        }
    }
}
```

### 2. DFS

좌표를 함수 파라미터로 처리하기 때문에 `Point`  필요x

```java
import java.util.*;

public class Main {
	static List<Integer> list = new ArrayList<>();
	static int[] dx = {-1, 1, 0, 0};
	static int[] dy = {0, 0, -1, 1};
	static int cnt = 0;
	
	public static void DFS(int x, int y, int[][] arr, int[][] ch, int n) {
		cnt++;
		ch[x][y] = 1;
		
		for(int i=0; i<4; i++) {
			int nx = x + dx[i];
			int ny = y + dy[i];
			
			if(nx >= 0 && nx < n && ny >= 0 && ny < n  && ch[nx][ny] == 0 && arr[nx][ny] == 1) {
				DFS(nx, ny, arr, ch, n);
			}
		}
	}
	
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[][] arr = new int[n][n];
        int[][] ch = new int[n][n];
        String[] temp = new String[n];
        for(int i=0; i<n; i++) {
        	temp[i] = sc.next();
        }
        for(int i=0; i<n; i++) {
        	String str = temp[i];
        	for(int j=0; j<n; j++) {
        		arr[i][j] = str.charAt(j) - '0';
        	}
        }
        
        int total = 0;
        List<Integer> list = new ArrayList<>();
        for(int i=0; i<n; i++) {
        	for(int j=0; j<n; j++) {
        		if(ch[i][j] == 0 && arr[i][j] == 1) {
        			total++;
        			DFS(i, j, arr, ch, n);
        			list.add(cnt);
        			cnt = 0;
        		}
        	}
        }
        
        Collections.sort(list);
        
        System.out.println(total);
        for(int i : list) {
        	System.out.println(i);
        }
    }
}
```

## 회고

재귀 호출을 할 때 항상 base case 먼저 생각을 했었다. 재귀라고 해서 항상 맨 위에 base case가 필요한 건 아니였다. 언제 필터링을 할 것인지가 중요하다. 즉, 호출 전에 필터링을 할 수도 있고, 호출 하고 필터링을 할 수도 있다. **함수가 안전한 상태로 호출되는지의 여부를 판단하자.**