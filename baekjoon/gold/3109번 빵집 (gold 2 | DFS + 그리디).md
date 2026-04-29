[백준 3109번 빵집](https://www.acmicpc.net/problem/3109)

## 문제 요약

첫째 열에서 마지막 열로 파이프를 연결하려고 함.

건물에는 놓을 수 없음.

각 칸은 오른쪽, 오른쪽 위 대각선, 오른쪽 아래 대각선으로 연결 할 수 있다.

여러개 설치할 건데 경로는 겹칠 수 없고, 서로 접할 수 없음. 즉, 하나의 파이프라인을 설치하면 더 이상 설치된 곳을 쓸 수 없음.

연결하는 파이프라인의 최대 개수 구하라.

## 핵심 조건 및 제약 사항

행 R → 1~10000
열 C → 5~500

. → 빈칸
x → 건물

## 시행착오

첫번째 열에서 시작하여 세방향으로 탐색하여 마지막 열로 가는 DFS.

마지막열에 도착했을때 탐색의 시작점을 첫번째 열에서 시작 안한 점으로 시작하기.

건물이나 파이프로 인하여 나아갈 수 없거나 마지막 열에 도달하지 못하는 경우 가지치기.

최대 개수를 못넘을 것 같으면 가지치기?
→ 이 판단을 하려면 앞으로 몇 개 더 만들 수 있는지 알아야 함.
→ 근데 경로가 서로 영향을 주기도 하고, 어떤 경로를 먼저 깔았느냐에 따라 뒤에 가능한 경로가 완전히 달라지기 때문에 남은 최대 개수를 정확히 계산할 수 없어서 못함.
→ 상황이 남은 걸 다 써도 못이긴다 할 때 사용하자!

각 행에서 하나씩 가능한 한 위쪽으로 붙어서 가는 그리디 전략 사용(오른쪽 위 대각선부터 탐색).
→ 만약 가능한 파이프 라인이 생성되면 되돌리지 않고 다음 시작점을 탐색한다.
→ 실패하더라도 되돌리지 않는다. 특정 위치에서 어떻게 해서든 오른쪽으로 도달하지 못하면 해당 위치에서 시작하는 모든 경로는 실패로 확정되므로 다른 경로를 통해서 와도 도달하지 못하는건 마찬가지이기 때문이다.

## 구현

```java
import java.util.*;
import java.io.*;

public class Main {
    static int r, c;
    static char[][] arr;
    static int answer;
    static int[] dx = {-1, 0, 1};
    static int[] dy = {1, 1, 1};
    static boolean flag;

    public static void DFS(int x, int y) {
        if (flag) return;
        if (y == c - 1) {
            answer++;
            flag = true;
        } else {
            for (int i = 0; i < 3; i++) {
                int nx = x + dx[i];
                int ny = y + dy[i];

                if (nx >= 0 && nx < r && ny >= 0 && ny < c && arr[nx][ny] != 'x' && !flag) {
                    arr[nx][ny] = 'x';
                    DFS(nx, ny);
                }
            }
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        r = Integer.parseInt(st.nextToken());
        c = Integer.parseInt(st.nextToken());

        arr = new char[r][c];

        for (int i = 0; i < r; i++) {
            String str = br.readLine();
            for (int j = 0; j < c; j++) {
                arr[i][j] = str.charAt(j);
            }
        }

        for (int i = 0; i < r; i++) {
            arr[i][0] = 'x';
            DFS(i, 0);
            flag = false;
        }

        System.out.println(answer);
    }
}
```

## 회고

초기에는 백트래킹처럼 모든 경로를 탐색하며 최적의 개수를 구하려 했지만, 경로들이 서로 영향을 주기 때문에 탐색 순서에 따라 결과가 달라질 수 있음을 깨달았다.

또한 "남은 경로로 최대 개수를 만들 수 있는지"를 기준으로 가지치기를 시도하려 했지만, 이후 가능한 경로 수를 정확히 예측할 수 없어 적용할 수 없었다.

이후 각 행에서 가능한 한 위쪽부터 경로를 선택하는 그리디 전략을 적용했다.

위쪽 경로는 선택지가 적기 때문에 먼저 확보해야 전체 파이프 개수를 최대로 만들 수 있다.

또한 한 번 방문한 칸은 성공 여부와 관계없이 다시 탐색하지 않도록 하였다. 특정 위치에서 이미 마지막 열까지 도달하지 못했다면, 다른 경로로 해당 위치에 도달하더라도 결과는 동일하게 실패하기 때문이다.

이를 통해 탐색을 크게 줄일 수 있었고, 전체 시잔 복잡도를 $O(R·C)$ 수준으로 낮출 수 있었다. (각 칸을 최대 한 번만 방문하기 때문에)

결론 → 경로를 찾는데, 그리디하게 찾는다.