## 문제 요약

응모자 아이디가 있고 불량 사용자 아이디가 있는데, 불량 사용자 아이디에 매핑 되는 경우의 수를 반환.

단, 목록이 같다면 하나로 센다. → 순서x

##  핵심 조건 및 제약 사항

응모자 → 1~8

불량 사용자 → 1 ~ 응모자 수

각 문자열 길이 → 1 ~ 8

## 시행착오

불량 사용자 아이디와 응모자 아이디 간의 매핑 여부를 기록하는 2차원 배열을 생성

만든 2차원 배열을 이용해 유저 조합을 구하려 했으나, 기존에 알던 조합 방식(`start` 인덱스를 뒤로 미루는 방식)을 사용하면 특정 유저가 선택되지 않는 경우가 발생함.

유저를 앞에서부터 차례대로만 뽑는(인덱스가 고정된) 일반적인 조합이 아니라, 제재 아이디 조건에 맞춰 유저를 앞뒤로 자유롭게 골라야 하기 때문에, 일단 순열로 모든 매칭을 구한 뒤 마지막에 중복을 제거하는 것이 맞다.

중복제거는 집합 사용.

이때 유저 방문 배열(`ch`)의 상태를 그대로 집합에 넣기 위해 비트마스킹 기법을 활용함.

탐색이 끝날 때마다 방문 체크된 유저들의 인덱스를 비트마스크 값으로 변환하여 `Set`에 저장함으로써 순서가 달라도 구성원이 같은 집합들을 하나로 퉁침.

중복이 모두 제거된 `Set`의 크기(`size()`)를 출력하여 해결함.

## 구현

```java
import java.util.*;

class Solution {
    static int[][] res;
    static int[] ch;
    static Set<Integer> set = new HashSet<>();
    
    static void dfs(int L, int banSize, int userSize){
        if(L == banSize){
            int bitmask = 0;
            for(int i=0; i<userSize; i++){
                if(ch[i] == 1){
                    bitmask |= 1 << i;
                }
            }
            
            set.add(bitmask);
            
            return;
        }
        else{
            for(int i=0; i<userSize; i++){
                if(res[i][L] == 1 && ch[i] == 0){
                    ch[i] = 1;
                    dfs(L+1, banSize, userSize);
                    ch[i] = 0;
                }
            }
        }
    }
    
    public int solution(String[] user_id, String[] banned_id) {
        int[] banCnt = new int[banned_id.length];
        
        ch = new int[user_id.length];
        res = new int[user_id.length][banned_id.length];
        
        for(int i=0; i<user_id.length; i++){
            String user = user_id[i];
            
            for(int j=0; j<banned_id.length; j++){
                String ban = banned_id[j];
                
                if(user.length() != ban.length()) continue;
                
                boolean flag = true;
                for(int k=0; k<user.length(); k++){
                    if(ban.charAt(k) == '*') continue;
                    if(user.charAt(k) != ban.charAt(k)){
                        flag = false;
                        break;
                    }
                }
                if(flag) res[i][j] = 1;
            }
        }
        
        dfs(0, banned_id.length, user_id.length);
          
        return set.size();
    }
}
```

## 회고

이번 문제를 풀면서 기존에 정형화된 알고리즘에 너무 얽매여 있었다는 걸 느꼈다.

수학적 정의로서의 조합을 코드로 구현하는 방법은 얼마든지 분리될 수 있음을 깨달았다.

탐색은 순열로 넓게 열어두고 자료구조와 비트연산으로 최종 형태를 제한하는(순서만 다른 중복된 결과를 제한하는) 유연한 접근법의 중요성을 배울 수 있었다.