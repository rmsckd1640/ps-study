## 문제 요약

손님들이 주문할 때 가장 많이 함께 주문한 단품메뉴들을 코스요리로 구성할거임.

개수에 맞춰 가장 많이 함께 주문된 단품메뉴 반환

## 핵심 조건 및 제약 사항

메뉴 최소 2가지 이상, 최소 2명 이상의 손님으로부터 주문된 것.

손님 별 주문 → 1 ~ 20

하나의 주문 안에 요리 개수 → 1 ~ 10

코스요리를 구성하는 단품메뉴들의 개수를 담아놓은 배열 → 1 ~ 10

사전 순으로 `오름차순` 정렬해서 return

## 시행착오

1. 조합으로 풀면 course배열을 돌며 각 개수별로 뽑는 방법

2. 아니면 몇 개든 다 뽑아 놓고 course배열에 맞추는 방법 → 부분집합

2번 선택.

각 선택된 경우를 map에 저장하여 course배열의 값(개수)와 문제 조건에 맞춰 답 추출

탐색전에 선택한 조합이 순서만 달라 다르게 들어가는 경우 방지 정렬 해놓고 탐색

## 구현

```java
import java.util.*;

class Solution {
    static HashMap<String, Integer> map = new HashMap<>();
    static List<Character> list = new ArrayList<>();
    
    static void dfs(int L, String str){
        if(L == str.length()){
            if(list.size() >= 2){
                StringBuilder sb = new StringBuilder();
                for(char c : list){
                    sb.append(c);
                }

                String temp = sb.toString();
                map.put(temp, map.getOrDefault(temp, 0) + 1);
                System.out.println(temp);
            }
            
            return;
        }
        
        list.add(str.charAt(L));
        dfs(L+1, str);
        list.remove(list.size()-1);
        dfs(L+1, str);
    }
    
    public String[] solution(String[] orders, int[] course) {
        
        List<String> answerList = new ArrayList<>();
        
        for(int i=0; i<orders.length; i++){
            char[] arr = orders[i].toCharArray();
            Arrays.sort(arr);
            String temp = new String(arr);
            dfs(0, temp);
        }
        
        for(int i=0; i<course.length; i++){
            int max = 0;
            
            //1. 첫번째 요리 개수에 맞춰 제일 많은 주문이 몇 개인지 확인
            for(Map.Entry<String, Integer> entry : map.entrySet()){
                if(entry.getKey().length() == course[i] && entry.getValue() > max){
                    max = entry.getValue();
                }
            }
            
            //2. 제일 많은 주문들 답에 추가(여러개일 수 있음)
            for(Map.Entry<String, Integer> entry : map.entrySet()){
                if(entry.getKey().length() == course[i] && entry.getValue() == max && entry.getValue() >= 2){
                    answerList.add(entry.getKey());
                }
            }
        }
        
        Collections.sort(answerList);
        
        String[] answer = answerList.toArray(new String[0]);
        
        return answer;
    }
}
```

## 회고

처음에는 모든 조합을 만든 뒤 `&& list.size() >= 2` 조건으로 걸러내려고 했다.

하지만 종료 조건(`return`)보다 바깥에서 처리하려 하다 보니, DFS가 끝까지 탐색한 뒤에도 제대로 되돌아가지 않는 문제가 있었다.

반대로 조건을 너무 일찍 검사하면 길이가 2 이상인 조합까지 탐색하지 못했다.

그래서 최종적으로는 DFS의 종료 시점(`L == str.length()`) 안에서 현재 조합의 길이를 검사하고, **`list.size() >= 2`인 경우만 `map`에 저장하도록 구현했다.**