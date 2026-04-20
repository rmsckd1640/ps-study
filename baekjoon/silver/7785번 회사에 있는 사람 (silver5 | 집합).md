[백준 7785번 회사에 있는 사람](https://www.acmicpc.net/problem/7785)

## 문제 요약

각 사람의 이름이 주어지고 "enter"는 출근, "leave"는 퇴근.

회사에 있는 사람의 이름을 사전 순의 역순으로 한 줄에 한명씩 출력.

## 핵심 조건 및 제약 사항

회사에는 동명이인이 없고, 대소문자가 다른 경우에는 다른 이름이다.

사람들의 이름은 알파벳 대소문자로 구성된 5글자 이하의 문자열

출입 기록의 수 n의 범위 2 ~ 1,000,000

## 시행 착오

"enter"인 경우 집합에 넣고 "leave"인 경우 집합에서 제거해야겠다. → 왜 집합을 썼지?

중복이 필요없고, 추가와 삭제가 `O(1)` 시간에 가능, 그리고 존재 여부 체크에 최적.

만약 `List`를 사용한다면 `remove()`메서드를 사용했을 때 `O(N)`이 걸리기 때문에 전체적으로 `O(N^2)`이 걸려 터진다. (입력 최대 1,000,000이기 때문)

집합을 사용하고 정렬까지 따로 하여 총 `O(NlogN)` 시간에 해결할 수 있다.

`List`에 옮겨 담고 역순으로 정렬해야겠다. → 이렇게 할 필요 없이 `TreeSet`을 역순으로 선언하면 된다.

결론은 `TreeSet`을 사용함으로써 삽입·삭제가 `O(logN)`이므로 총 `O(NlogN)` 시간에 해결할 수 있으며 자동으로 정렬이 유지된다.

## 구현

```java
import java.util.*;
import java.io.*;

public class Main{
	public static void main(String[] args) throws IOException{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
		
//		Set<String> set = new HashSet<>();
		Set<String> set = new TreeSet<>(Collections.reverseOrder());
		
		for(int i=0; i<n; i++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			String person = st.nextToken();
			String status = st.nextToken();
			
			if(status.equals("enter")) set.add(person);
			else {
//				if(set.contains(person)) set.remove(person);
				set.remove(person); //Set에서 없는 값 remove 해도 그냥 무시됨
			}
		}
		
//		List<String> list = new ArrayList<>(set);
//		list.sort(Collections.reverseOrder());
		StringBuilder sb = new StringBuilder();
		for(String str : set) {
			sb.append(str).append("\n");
		}
		
		System.out.print(sb.toString().trim());
	}
}
```

## 회고

이 문제를 풀면서 왜 집합을 사용했는지 생각을 하지 않고 그냥 써버렸다. 자료구조를 사용할 때는 이유를 생각하며 사용하자.

이 문제는 '현재 상태 집합'을 관리하는 문제이기 때문에 `Set`을 사용한다.

`HashSet`을 사용할 경우 삽입/삭제는 `O(1)`이지만 정렬을 따로 수행해야 한다.
`TreeSet`은 삽입/삭제가 `O(logN)`이지만 자동으로 정렬이 유지된다.