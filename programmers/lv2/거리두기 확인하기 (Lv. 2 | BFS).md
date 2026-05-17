## 문제 요약

5x5 크기 대기실에 응시자들이 앉을 수 있는데 맨해튼 거리가 2이하로 붙어 앉을 수 없음.

사이가 파티션으로 막혀 있을 경우에는 허용함.

5개의 대기실 각각 거리두기를 잘하고 있으면 1, 지키지 않고 있으면 0을 담아 return

## 핵심 조건 및 제약 사항

대기실 개수가 총 5개, 대기실 크기가 5x5 → 완전탐색해도 충분히 통과

## 시행착오

각 대기실마다 주어진 입력을 배열로 가공하면서 응시자의 위치를 리스트에 담았다.

응시자별로 BFS기반 탐색을 하여 깊이 2까지 갔을 때 응시자가 없다면 거리두기를 잘 지킨 것이다.

파티션 주변은 볼 필요도 없으니(안전하니깐) 탐색하지 않는다.

응시자를 발견하면 false를 return한다.

## 구현

```java
import java.util.*;

class Point{
    int x;
    int y;
    
    public Point(int x, int y){
        this.x = x;
        this.y = y;
    }
}

class Solution {
    static char[][] arr;
    static int[] dx = {-1,1,0,0};
    static int[] dy = {0,0,-1,1};
    static int[][] ch;
    
    static boolean bfs(Point p){
        Deque<Point> q = new ArrayDeque<>();
        q.offer(p);
        ch[p.x][p.y] = 1;
        
        for(int i=0; i<2; i++){
            int size = q.size();
            
            for(int j=0; j<size; j++){
                Point cur = q.poll();
                
                for(int k=0; k<4; k++){
                    int nx = cur.x + dx[k];
                    int ny = cur.y + dy[k];
                    
                    if(nx >= 0 && nx < 5 && ny >= 0 && ny < 5 && arr[nx][ny] != 'X' && ch[nx][ny] == 0){
                        if(arr[nx][ny] == 'P') return false;
                        ch[nx][ny] = 1;
                        q.offer(new Point(nx, ny));
                    }
                }
            }
        }
        
        return true;
    }
    
    public int[] solution(String[][] places) {
        int[] answer = new int[5];
        
        for(int i=0; i<5; i++){
            arr = new char[5][5];
            List<Point> list = new ArrayList<>();
            boolean flag = false;
            for(int j=0; j<5; j++){
                for(int k=0; k<5; k++){
                    arr[j][k] = places[i][j].charAt(k);
                    if(arr[j][k] == 'P') list.add(new Point(j, k));
                }
            }
            
            for(Point p : list){
                ch = new int[5][5];
                if(!bfs(p)){
                    flag = true;
                    break;
                }
            }
            
            if(!flag) answer[i] = 1;
        }
        
        return answer;
    }
}
```

## 회고

각 탐색마다 방문배열 초기화하는거 잊지 말기!