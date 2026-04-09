[백준 2346번 풍선 터뜨리기](https://www.acmicpc.net/problem/2346)

## 문제 요약

1번부터 n번까지 n개의 풍선이 원형으로 놓여있고, 각 풍선을 터뜨리고 안에 있는 정수 값만큼 이동하는 문제.

## 핵심 조건 및 제약 사항

정수가 양수이면 오른쪽, 음수이면 왼쪽이동.
1 <= n <= 1000

## 시행착오

1. 원형배열

정적배열을 나머지 연산을 통해 원형으로 이용하자.
→ 삭제시 처리가 힘들기 때문에 리스트의 `remove()`메서드 활용해야겠다.

리스트를 이용해도 변화되는 인덱스에 맞추기 힘듬.
→ 번호와 값을 저장하는 클래스를 만들어 이용하자.

오른쪽 이동 시 삭제하면 한칸씩 당겨와지므로 -1
왼쪽 이동 시에는 영향 없음.
→ 둘이 나눠서 처리해야함.

2. Deque

인덱스 관리가 힘듬.
→ 인덱스를 계산하는 대신, `Deque`의 양방향 삽입/삭제를 이용해 회전시키며 해결.

## 구현

### 1. 원형배열

```java
import java.util.*;

class Balloon{
	int id; //풍선 번호
	int val; //그 안에 값
	
	public Balloon(int num, int val) {
		this.id = num;
		this.val = val;
	}
}

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        
        List<Balloon> list = new ArrayList<>();
        
        for(int i=1; i<=n; i++) {
        	list.add(new Balloon(i, sc.nextInt()));
        }
        
        StringBuilder sb = new StringBuilder();
        int cur = 0;
        while(true) {
        	//현재 풍선 터트리고 번호보기
        	Balloon b = list.remove(cur);
        	sb.append(b.id).append(" "); //현재 번호 추가
        	
        	if(list.isEmpty()) break; //더이상 풍선이 없다면 끝
        	
        	int move = b.val;
        	int size = list.size(); //삭제된 후의 크기
        	
        	if(move > 0) cur = (cur + move - 1) % size;
        	else {
        		cur = (cur + move) % size;
        		if(cur < 0) cur += size;
        	}
        }

        System.out.println(sb.toString().trim()); //맨 뒤 공백 제거
    }
}
```

### 2. Deque

```java
import java.util.*;

class Balloon{
	int id; //풍선 번호
	int val; //그 안에 값
	
	public Balloon(int num, int val) {
		this.id = num;
		this.val = val;
	}
}

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        
        Deque<Balloon> deq = new ArrayDeque<>();
        
        for(int i=1; i<=n; i++) {
        	deq.addLast(new Balloon(i, sc.nextInt()));
        }
        
        StringBuilder sb = new StringBuilder();
        
        while(true) {
        	//항상 맨앞의 풍선을 터뜨림
        	Balloon b = deq.pollFirst();
        	sb.append(b.id).append(" ");
        	
        	//터뜨리고 난 후 풍선이 없으면 break
        	if(deq.isEmpty()) break;
        	
        	int move = b.val;
        	
        	//삭제했을때 오른쪽으로 가는 경우는 이미 한칸씩 댕겨온거니깐 move-1 만큼
        	if(move > 0) {
        		for(int i=0; i<move-1; i++) {
        			deq.addLast(deq.pollFirst());
        		}
        	}
        	
        	//왼쪽으로 가는 경우는 영향 안받음
        	else {
        		for(int i=0; i<Math.abs(move); i++) {
        			deq.addFirst(deq.pollLast());
        		}
        	}
        }
        
        System.out.println(sb.toString().trim());
    }
}
```

## 회고

반복문의 종료 조건과 내부 로직을 한 번에 설계하려다 보니 구현이 잘 되지 않았다. 로직이 복잡하거나 내 생각대로 되지 않는다면 `while(true)`로 먼저 구조를 잡고, 발생 가능한 예외 상황을 검토하며 탈출(`break`) 조건을 역으로 설계하는 방식을 시도해 보자.