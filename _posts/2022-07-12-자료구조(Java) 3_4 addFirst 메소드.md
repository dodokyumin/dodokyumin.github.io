## addFirst 메소드

### Linked List 맨 앞에 추가하는 경우

리스트가 있고, 연결 리스트의 생성자를 호출할 때

가장 먼저 head를 null로 지정한다.

currentSize는 증가시킬지 감소시킬지만 염두하자.

 1️⃣

![image-20220713171801429](../images/2022-07-12-자료구조(Java) 3_4 addFirst 메소드/image-20220713171801429.png)

head는 null로 생성된다.

Linked List 에 새로운 요소를 추가할 때, 노드 클래스 객체 A를 하나 만들고,

<br>

2️⃣

값이 null인 head를 가져와 새로 생성된 노드A 를 가리키도록 만든다.

![image-20220713172226068](../images/2022-07-12-자료구조(Java) 3_4 addFirst 메소드/image-20220713172226068.png)

<br>

3️⃣

새로운 노드B 를 생성하여 맨 앞으로 넣어줄 때,

![image-20220713172412184](../images/2022-07-12-자료구조(Java) 3_4 addFirst 메소드/image-20220713172412184.png)

새로운 노드B의 next도 A를 가리키도록 한다.

만약 head먼저 노드B를 가리키도록 만든다면, **링크가 끊긴(받는 포인터가 한개도 없는) 노드 A는 가비지 컬렉션(Garbage collection)**이 일어난다.

<br>

4️⃣

![image-20220713173027658](../images/2022-07-12-자료구조(Java) 3_4 addFirst 메소드/image-20220713173027658.png)

Head를 새 노드 B를 가리키게 한다. 이후 맨 앞에 추가되는 요소들도 마찬가지 과정을 거쳐서 추가된다.

<br>

<br>

다음 요소를 추가할 때 Head가 어디 있는지 알기 때문에 단 한개의 요소만 확인하면 된다.

(새로운 요소를 추가할 때 기존의 요소들 모두를 확인할 필요가 없다.)

**👉 Liked List의 맨 앞에 요소를 추가하는 작업의 시간 복잡도는 정확히 1이 된다.**

<br>

<br>

### addFirst메소드 코드 뜯어보기

```java
public void addFirst(E obj){
	Node<E> node = new Node<E>(obj); // 1
	node.next = head; // 2
	head = node; // 3
    currentSize ++; //새로운 요소를 리스트에 추가하였으니 ++해준다.
}
```

❗ 순서가 매우 중요하다. 

<br>

1️⃣ 우선적으로 새로운 노드 생성하기.

```java
Node<E> node = new Node<E>(obj);
```

(obj) 는 그저 메소드 파라미터로 받은 제너릭 타입의 obj를 받아 Node 클래스로 넘기는 것 뿐.

Node 클래스는 이 객체(obj)를 data에 저장하고 next는 null을 가리키게 한다.

<br>

2️⃣3️⃣ 기존의 head가 가리키는 노드를 새 노드가 가리키게 만든다.(없다면 null)

```java
node.next = head; 
```

- 처음 노드를 생성하는 경우도 상관 없다.

A의 next는 null을 가리키고, 그다음 head는 next를 가리킨다.

![image-20220713175643491](../images/2022-07-12-자료구조(Java) 3_4 addFirst 메소드/image-20220713175643491.png)

- 리스트가 비어있지 않은 경우.

새로운 노드를 가지고 node.next가 리스트의 (기존의 )첫 노드를 가리키도록 한다.

head가 가리키는 곳과 메모리상 같은 위치.

<br>

4️⃣ head포인터가 새로운 노드를 가리키도록 한다.

```java
head = node; // 3
```

<br>

👉 **위 코드는  5가지 경계 조건에 대하여 생각하였을 때에도 문제가 없다. 그리고 새로운 요소를 추가하기 위해 뒷부분을 살펴볼 필요가 없기 때문에 시간 복잡도는 1이다.**

<br>

<br>

<br>

## 생각해보기

1\) head가 비어있는 경우, 즉 head가 null을 가리키는 경우에 addFirst 메소드를 사용하면 node.next, head가 어떻게 달라지나요?



node.next는 생성시부터 null을 가리키고 있었고, addFirst로 연결리스트에 들어간 이후에도 null을 가리킨다.

head는 null을 가리키고 있다가 방금 막 삽입된 node를 가리키게 된다.

<br>

<br>

<br>

## References

> 자바로 구현하고 배우는 자료구조 -Rob Edwards (boostcourse) 





