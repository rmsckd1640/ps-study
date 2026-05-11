## 문제 요약

N개의 회의가 있고, 각 회의의 시작시간, 종료시간이 주어짐.

회의실이 하나가 있고, 하루동안 회의를 최대한 많이 배정하고 싶음.

배정 가능한 최대 회의 수, 배정한 회의의 번호를 시간대순으로 출력.

## 핵심 조건 및 제약 사항

회의의 수 N → 5~500

한 회의에서 시작시간과 종료시간이 같은 경우는 주어지지 않음.

종료시간과 시작시간이 같은 경우에는 시간이 겹친다고 말하지 않음.

## 시행착오

종료시간이 빠르면 빠를수록 많이 배정할 수 있음. → 종료시간 기준 오름차순

## 구현

```java
import java.util.*;

class Meeting{
	int num;
	int in;
	int out;
	
	public Meeting(int num, int in, int out) {
		this.num = num;
		this.in = in;
		this.out = out;
	}
}

public class Main{
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		int n = sc.nextInt();
		
		Meeting[] arr = new Meeting[n];
		
		for(int i=0; i<n; i++) {
			arr[i] = new Meeting(sc.nextInt(), sc.nextInt(), sc.nextInt());
		}
		
		Arrays.sort(arr, (a,b) -> {
			if(a.out == b.out) return a.in - b.in; //상관없지만 안전하게 끝 시간 같은 경우
			return a.out - b.out;
		});
		
		StringBuilder sb = new StringBuilder();
		int end = arr[0].out;
		sb.append(arr[0].num).append(" ");
		int cnt = 1;
		
		for(int i=1; i<n; i++) {
			if(end <= arr[i].in) {
				end = arr[i].out;
				sb.append(arr[i].num).append(" ");
				cnt++;
			}
		}
		
		System.out.println(cnt);
		System.out.print(sb.toString().trim());
	}
}
```