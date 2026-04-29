[SWEA 5215번 햄버거 다이어트](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWT-lPB6dHUDFAVT)

## 문제 요약

재료의 수, 제한 칼로리가 주어짐.

각 재료마다 재료의 점수와 칼로리가 주어짐.

제한 칼로리를 넘지 않으면서 맛에 대한 점수가 높게 조합 해야함.

## 핵심 조건 및 제약 사항

재료의 수 N 1~20
제한 칼로리 L 1 ~ 10000
맛에 대한 점수 T, 재료의 칼로리 K 1~1000

## 시행착오

어떤 재료를 선택했을 때 칼로리를 보고 또 다른 재료를 선택할 수 있는지 확인. → 부분집합

## 구현

시간복잡도는 $O(2^n)$이지만 칼로리 초과 시 가지치기로 평균 수행 시간은 감소한다.

```java
import java.util.*;

class Burger {
    int score;
    int cal;

    public Burger(int score, int cal) {
        this.score = score;
        this.cal = cal;
    }
}

public class Solution {
    static int n;
    static int l;
    static Burger[] arr;
    static int sumCal;
    static int sumScore;
    static int answer;

    public static void DFS(int L) {
        if (sumCal > l) {
            return;
        }

        if (L == n) {
            answer = Math.max(answer, sumScore);
            return;
        } else {
	        // 중간 상태에서 갱신할 필요 없음.  
			// DFS는 각 재료를 선택/비선택하여 모든 경우를 끝(L == n)까지 탐색하므로,  
			// 일부만 선택한 경우도 결국 끝까지 내려가면서 모두 평가됨.  
			// 따라서 answer는 L == n에서만 갱신해도 충분함.
            //answer = Math.max(answer, sumScore);

            sumCal += arr[L].cal;
            sumScore += arr[L].score;
            DFS(L + 1);

            sumCal -= arr[L].cal;
            sumScore -= arr[L].score;
            DFS(L + 1);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int T = sc.nextInt();

        for (int tc = 1; tc <= T; tc++) {
            answer = 0;
            sumCal = 0;
            sumScore = 0;

            n = sc.nextInt();
            l = sc.nextInt();

            arr = new Burger[n];
            ch = new boolean[n];

            for (int i = 0; i < n; i++) {
                arr[i] = new Burger(sc.nextInt(), sc.nextInt());
            }

            DFS(0);

            System.out.println("#" + tc + " " + answer);
        }
    }
}
```

## 추가 구현

각 재료를 선택하거나 선택하지 않는 부분집합 문제이지만, 칼로리라는 제한 조건이 존재하고, 점수의 최댓값을 구해야 하므로 상태를 정의하여 중복 계산을 줄일 수 있는 0/1 Knapsack(DP) 문제로 해결하는 것이 좋다.

각 재료마다 모든 칼로리 범위(0 ~ L)를 한 번씩 확인하며 DP 배열을 갱신하므로, 전체 시간복잡도는 $O(n·l)$이다.

```java
import java.util.*;

class Burger {
    int score;
    int cal;

    public Burger(int score, int cal) {
        this.score = score;
        this.cal = cal;
    }
}

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int T = sc.nextInt();

        for (int tc = 1; tc <= T; tc++) {
            int n = sc.nextInt(); // 재료 개수
            int l = sc.nextInt(); // 제한 칼로리

            Burger[] arr = new Burger[n];
            for (int i = 0; i < n; i++) {
                arr[i] = new Burger(sc.nextInt(), sc.nextInt());
            }

            int[] dp = new int[l + 1];

            for (int i = 0; i < n; i++) {
                for (int j = l; j >= arr[i].cal; j--) {
                    dp[j] = Math.max(dp[j], arr[i].score + dp[j - arr[i].cal]);
                }
            }

            System.out.println("#" + tc + " " + dp[l]);
        }
    }
}

// dp[j]: 칼로리 j 이하에서 얻을 수 있는 최대 점수
// 현재 재료를 선택하는 경우를 고려하여 갱신
// 역순 순회로 각 재료는 한 번만 사용
```