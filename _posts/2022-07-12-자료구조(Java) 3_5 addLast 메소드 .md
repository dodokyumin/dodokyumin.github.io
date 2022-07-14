## addLast 메소드

Linked List 맨 뒤에 요소를 추가하는 경우

![image-20220714181404978](../images/2022-07-14-자료구조(Java) 3_5 addLast 메소드 /image-20220714181404978.png)

새로운 노드 D를 추가할 때, head에서부터 올라가는 것은

```java
head.next.next.next... = node
```

매우 낭비적이고 확장성이 떨어지는 일이다.

그러므로,

👉 **임시포인터(Temporary Pointer)** 를 활용하여, 임시포인터가 연결 리스트의 마지막을 가리키도록 한다.



### 임시포인터 Temporary Pointer

**HOW?**

![image-20220714182213502](../images/2022-07-14-자료구조(Java) 3_5 addLast 메소드 /image-20220714182213502.png)

1️⃣ 임시포인터(tmp)를 만들어 head 부터 가리키기 시작한다.



2️⃣ .next가 노드가 아닌 NULL 일 때, 그 노드가 해당 리스트의 마지막 노드이므로,

head부터 시작한 tmp가 각 노드 (A, B, C)를 거치며 조건을 던진다.

👉**.next 변수가 null을 향해 가리키고 있는가?**

true 면 현 위치의 노드가 마지막 노드. false이면 다음 노드로 넘어가게 한다.



3️⃣ 다음 노드로 어떻게 넘어갈까?

```java
tmp = tmp.next;
```

.next(다음 노드)의 값을 현 tmp에 저장해준다. 자동적으로 기존 tmp의 링크는 끊어지게 된다.



4️⃣ 리스트 마지막에 도착했다면?

```java 
temp.next = node;
```

temp.next는 NULL이 되고, temp.next를 null 대신 새로운 노드를 가리키게 하면 된다.



결론적으로 메소드 코드는 이렇게 된다.

<br>

<br>

### addLast 메소드 기본 동작 코드

```java
public void addLast(E obj){
	Node<E> tmp = head;
	while(tmp.next != null)
		tmp=tmp.next
	tmp.next=node;
}
```

이제 메소드의 끝에 도착하면 어떻게 될까?

tmp는 범위를 벗어나며 가비지 컬렉션(Garbage Collection)이 된다.

<br>

<br>



------

### 문제 1. 경계 조건

#### 만약 리스트가 비어있고 head가 null을 가리킨다면?

![image-20220714192106947](../images/2022-07-14-자료구조(Java) 3_5 addLast 메소드 /image-20220714192106947.png)

```java
Node<E> tmp = head; //head값이 null, tmp도 null을 가리키게 됨
while(tmp.next != null) //tmp.next 즉, null의 next를 찾게되고 이때 에러 발생!
		tmp=tmp.next
```

tmp가 현재 null을 가리키고 있을 때, tmp.next는 

**NullPointerException** 이라는 런타임 에러를 발생시킨다.



⛔NullPointerException은 **어떤 값이 null인데 그것과 관련된 변수를 접근하려는 상황**이다.

​		그러므로 비어있는 리스트 맨 뒤에 요소를 추가할 때에 예외처리를 해주어야 한다.



#### NullPointerException 예외처리를 추가한 addLast 메소드 코드

```java
public void addLast(E obj){
	Node<E> node = new Node<E>(obj);
	if (head == null){ // head가 비어있는 경우
		head=node; //head가 이제 추가하려는 새로운 node를 가리키게 하고
		currentsize++;
		return; //return을 사용하여 else문 필요없이 이 메소드를 나오게 한다.
	}
	Node<E> tmp = head;
	while(tmp.next != null)
		tmp=tmp.next
	tmp.next=node;
	currentsize++;
}
```

<br>

<br>

### 문제 2. 시간 복잡도

위 코드는 새 노드를 추가하기 위한 위치를 알아볼 때까지

리스트의 요소 개수만큼 while 반복문이 돈다.

👉**시간 복잡도는 O(n)이 된다.**



#### tail 포인터 사용하기

![image-20220714200046526](../images/2022-07-14-자료구조(Java) 3_5 addLast 메소드 /image-20220714200046526.png)

tail 이라는 새로운 포인터를 만들고 반대쪽 끝에 둔다면,

👉마지막 요소의 next를 부르려면 단순히 **tail.next**라고 하면 된다.



#### ✨tail 포인터를 사용한 최종 addLast 메소드 코드

```java
public void addLast(E obj){
	Node<E> node = new Node<E>(obj);
	if (head == null){
		head=node;
		tail=node; // head 포인터뿐만 아니라 tail 포인터도 바꿔줘야 한다.
		currentsize++;
		return;
	}
	tail.next=node; //tail의 next가 현재 추가하려는 새로운 노드를 가리키게 한다.
	tail = node; //요소를 뒤에 추가했다면, 추후 추가할 노드를 받기 위하여 tail 포인터도 다시 마지막으로 옮겨줘야 한다.
	currentsize++;
}
```



head가 null일 때 tail도 마찬가지로 null이기 때문에,

![image-20220714200544497](../images/2022-07-14-자료구조(Java) 3_5 addLast 메소드 /image-20220714200544497.png)

```java
head=node;
```



☝ 그리고 전역 범위의 변수 tail을 만들어야한다.

리스트의 마지막을 가리키는 tail 포인터를 head, currentSize와 같은 전역 변수로 설정한다.

👉tail 포인터 사용을 위해 앞 글의 addFirst 메소드에 tail 코드도 추가해주어야 한다.

**LinkedList를 생성할 때 tail 도 head 처럼 null을 가리키게 한다.**

**List가 비어있으면 요소를 추가했을 때 그 요소가 첫째이자 마지막이므로 addFirst 메소드에도 tail 포인터를 사용한다.**

(LinkedList의 내부 클래스는 전역 변수가 세개 있다. head, currentSize, tail)



tail포인터를 쓰면 복잡도가 올라간다. (노드를 추가하거나 삭제할 때마다 작업량이 늘어나므로)

하지만 더 효율적으로 바뀐다.

👉 tail 포인터를 사용하는 메소드는 **시간 복잡도가 O(1)** (상수시간)이 된다.

<br>

<br>

<br>

## 생각해보기

1\) 왜 currentSize 변수 대신 tail 포인터를 사용하나요?



마지막 노드를 찾을 때 head부터 접근할시 시간복잡도가 O(n)이 되지만, 리스트의 마지막을 가리키는 tail 포인터를 사용하면 즉시 마지막 요소 뒤에 새로운 노드를 삽입할 수 있으므로 시간복잡도를 O(1)로 줄일 수 있다.

<br>

<br>

<br>

## References

> 자바로 구현하고 배우는 자료구조 -Rob Edwards (boostcourse) 
