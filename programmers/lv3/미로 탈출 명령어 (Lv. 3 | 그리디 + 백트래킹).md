## 문제 요약

S에서 시작해서 E로 탈출하는데 이동해야 하는 거리 k를 꼭 채워야함.

이동경로를 문자열로 나타내는데 사전 순으로 가장 빠른 경로로 탈출 해야함.

## 핵심 조건 및 제약 사항

좌측 상단 (0,0)이 아니라 (1,1)

k → 1 ~ 2500

## 시행착오

l r u d를 이동하는데 사전 순으로 가장 빠른 경로로 탈출 해야하니 d l r u 순으로 탐색하면 최종적으로 사전 순으로 빠른 것 부터 탐색하게 됨.

백트래킹으로 탐색하면 될 것 같은데 최대 이동 횟수가 2500번이라 가지치기를 잘해야 될 것 같음.

1. 탐색이 되는 순간 사전 순 가장 빠른 것이 탐색이 된 것이니 탐색 됐으면 끝내기.
2. 이동하다가 그 위치에서 탈출까지의 거리가 남은 이동 횟수보다 크다면 도달할 수 없으니 가지치기.
3. 만약 이동횟수가 다 채워지지 않고 탈출칸에 도착했을때 남은 이동 횟수가 짝수여야 왔다갔다 해서 다시 탈출에 도착할 수 있음.

## 구현

```java
import java.util.*;

class Solution {
    
    //사전 순 탐색 d l r u
    static int[] dx = {1,0,0,-1};
    static int[] dy = {0, -1, 1, 0};
    static int endR;
    static int endC;
    static int cntK;
    static char[] arr;
    static String answer = "";
    static int N;
    static int M;
    
    
    
    static void DFS(int x, int y, int cnt){
        //답이 이미 구해졌으면
        if(answer != "") return;
        
        //남은 횟수보다 남은 최단 거리가 더 큰 경우
        //남은 최단 거리 계산 (맨해튼 거리)
        int dist = Math.abs(x - endR) + Math.abs(y - endC);
        if(dist > cntK - cnt) return;
        
        //남은 최단거리로 도착했을 때 남은 횟수가 짝수여야 왕복할 수 있음. 아니면 가지치기.
        if((cntK - cnt - dist) % 2 != 0) return;
        
        if(cnt == cntK){
            if(x == endR && y == endC) answer = new String(arr);
            return;
        }
        
        else{
            for(int i=0; i<4; i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                
                if(nx >= 0 && nx < N && ny >= 0 && ny < M){
                    if(i == 0) arr[cnt] = 'd';
                    else if(i == 1) arr[cnt] = 'l';
                    else if(i == 2) arr[cnt] = 'r';
                    else if(i == 3) arr[cnt] = 'u';
                    
                    DFS(nx, ny, cnt+1);
                }
            }
        }
    }
    
    public String solution(int n, int m, int x, int y, int r, int c, int k) {
        
        N = n;
        M = m;
        endR = r-1;
        endC = c-1;
        cntK = k;

        arr = new char[k];
        
        DFS(x-1, y-1, 0);
        
        if(answer == "") return "impossible";
        return answer;
    }
}
```

## 회고

사전 순으로 탈출해야 한다는 것을 보면 처음부터 그렇게 탐색해 보자 하고 생각하기.

백트래킹 구현 시 가지치기 많이 생각해 보기.