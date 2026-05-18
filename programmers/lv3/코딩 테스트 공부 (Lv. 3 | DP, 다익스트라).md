## 문제 요약

문제를 풀기위한 알고력, 코딩력이 있음.

모든 문제를 풀기위한 알고력, 코딩력을 갖추기 위한 최소 시간 구하기

알고력 공부해서 1 올리는데 시간 1

코딩력 공부해서 1 올리는데 시간 1

만약 어떤 문제를 풀 수 있다면 그 문제를 푸는 시간과 풀었을 때 상승하는 알고력, 코딩력이 주어짐.

## 핵심 조건 및 제약 사항

알고력과 코딩력 → 0 ~ 150

문제 개수 → 100

문제마다 풀었을 때 증가하는 폭 → 0 ~ 30

문제 푸는데 드는 시간 → 1 ~ 100


## 시행착오

매 순간 선택지가 존재함 -> 완전탐색? -> 경우의 수가 너무 많음 -> DP?

10 15인 문제를 풀려면

10 10 에서 공부로 -> 10 15
10 10 에서 문제풀이로 -> 10 15

아 전에 어떻게 왔든 상관없이 현재 상태가 중요한거구나!

그럼 i/j에 도달하는 최소 시간을 저장하며 쌓아 올라가자 -> DP

## 구현(DP)

dp 배열의 크기 만큼 돌며 알고 공부, 코딩 공부, 문제 풀이 총 최대 102번의 연산을 한다.

총 시간 복잡도 → $O(maxAlp · maxCop · 102)$ → 230만

```java
import java.util.*;

class Solution {
    public int solution(int alp, int cop, int[][] problems) {
        
        //문제들 중에서 가장 큰 알고력, 코딩력 뽑기
        int maxAlp = 0;
        int maxCop = 0;
        for(int problem[] : problems){
            maxAlp = Math.max(maxAlp, problem[0]);
            maxCop = Math.max(maxCop, problem[1]);
        }
        
        //초기가 더 크면 나중에 초기화 시킬 때 에러나기 때문에 예외처리(모든 문제를 풀 수 있는 자격이 이미 있음)
        if (alp >= maxAlp && cop >= maxCop) {
            return 0;
        }
        //하나씩만 더 큰 경우 목표치로 낮춰서 잡기 
        alp = Math.min(maxAlp, alp);
        cop = Math.min(maxCop, cop);
        
        
        //가장 큰 알고력, 코딩력 기준 dp배열 선언 및 초기화
        int[][] dp = new int[maxAlp+1][maxCop+1];
        for(int[] arr : dp){
            Arrays.fill(arr, 1_000_000_000);
        }
        dp[alp][cop] = 0;
        
        //상태 전이 시작
        for(int i=0; i<=maxAlp; i++){
            for(int j=0; j<=maxCop; j++){
                
                //1. 알고력 (+1 했을때 인덱스 넘어가는거 방지)
                if(i+1 <= maxAlp) dp[i+1][j] = Math.min(dp[i+1][j], dp[i][j]+1);
                
                //2. 코딩력
                if(j+1 <= maxCop) dp[i][j+1] = Math.min(dp[i][j+1], dp[i][j]+1);
                
                //3. 문제풀기
                for(int[] arr : problems){
                    //풀 수 있는지 확인
                    if(i >= arr[0] && j >= arr[1]){
                        //능력치가 초과할경우 위에 처럼 방지해서 안푸는게 아니라 풀긴해야하니깐 풀엇을 때 상한선을 지정해서 인덱스 범위 초과 에러 방지
                        int tempAlp = Math.min(i+arr[2], maxAlp);
                        int tempCop = Math.min(j+arr[3], maxCop);
                        
                        dp[tempAlp][tempCop] = Math.min(dp[tempAlp][tempCop], dp[i][j] + arr[4]);
                    }
                }
            }
        }
        
        return dp[maxAlp][maxCop];
    }
}
```

## 추가 구현(다익스트라)

alp, cop을 시작점으로 maxAlp, maxCop을 도착점으로 계속 최단거리를 갱신해 나가는 다익스트라 풀이.

정점 수 → 150x150 = 22500

간선 수 → 알고력 공부하기 1개, 코딩력 공부하기 1개, 문제 풀기(문제집의 개수 만큼 발생) P개

문제에서 주어진 최대 문제수 100개

정점 하나당 1+1+100개의 간선 발생 → 간선수 = 22500 x 102 

총 시간복잡도 $O(ElogE)$ → 5천만

```java
import java.util.*;

class Edge implements Comparable<Edge>{
    int alp;
    int cop;
    int cost;
    
    public Edge(int alp, int cop, int cost){
        this.alp = alp;
        this.cop = cop;
        this.cost = cost;
    }
    
    @Override
    public int compareTo(Edge e){
        return this.cost - e.cost;
    }
}

class Solution {
    public int solution(int alp, int cop, int[][] problems) {
        
        //문제들 중에서 가장 큰 알고력, 코딩력 뽑기
        int maxAlp = 0;
        int maxCop = 0;
        for(int problem[] : problems){
            maxAlp = Math.max(maxAlp, problem[0]);
            maxCop = Math.max(maxCop, problem[1]);
        }
        
        //초기가 더 크면 나중에 초기화 시킬 때 에러나기 때문에 예외처리(모든 문제를 풀 수 있는 자격이 이미 있음)
        if (alp >= maxAlp && cop >= maxCop) {
            return 0;
        }
        //하나씩만 더 큰 경우 목표치로 낮춰서 잡기 
        alp = Math.min(maxAlp, alp);
        cop = Math.min(maxCop, cop);
        
        int[][] dis = new int[maxAlp+1][maxCop+1];
        for(int[] arr : dis){
            Arrays.fill(arr, 1_000_000_000);
        }
        dis[alp][cop] = 0;
        
        PriorityQueue<Edge> pq = new PriorityQueue<>();
        pq.offer(new Edge(alp, cop, 0));
        while(!pq.isEmpty()){
            Edge cur = pq.poll();
            
            //가장 어려운 문제를 만족하는게 나왔다 -> 그리디하게 뽑았으니 지금이 최소 시간이다 바로 return
            if(cur.alp >= maxAlp && cur.cop >= maxCop){
                return cur.cost;
            }
            
            //다음 가는데 드는 비용이 이미 그자리 비용보다 크면 볼 필요 없음
            if(cur.cost > dis[cur.alp][cur.cop]) continue;
            
            //1. 알고력 공부하기
            if(cur.alp + 1 <= maxAlp){
                if(dis[cur.alp+1][cur.cop] > cur.cost+1){
                    dis[cur.alp+1][cur.cop] = cur.cost+1;
                    pq.offer(new Edge(cur.alp+1, cur.cop, cur.cost+1));
                }
            }
            
            //2. 코딩력 공부하기
            if(cur.cop + 1 <= maxCop){
                if(dis[cur.alp][cur.cop+1] > cur.cost+1){
                    dis[cur.alp][cur.cop+1] = cur.cost+1;
                    pq.offer(new Edge(cur.alp, cur.cop+1, cur.cost+1));
                }
            }
            
            //3. 문제풀기
            for(int[] pro : problems){
                if(cur.alp >= pro[0] && cur.cop >= pro[1]){ //풀 수 있다면
                    //여기서도 문제를 풀었을때 넘어가면 안되는데 풀긴해야하니 상한선 지정
                    int tempAlp = Math.min(cur.alp+pro[2], maxAlp);
                    int tempCop = Math.min(cur.cop+pro[3], maxCop);
                    
                    if(dis[tempAlp][tempCop] > cur.cost + pro[4]){
                        dis[tempAlp][tempCop] = cur.cost + pro[4];
                        pq.offer(new Edge(tempAlp, tempCop, cur.cost + pro[4]));
                    }
                }
            }
        }
        
        return dis[maxAlp][maxCop];
    }
}
```