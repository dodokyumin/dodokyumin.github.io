## Java 메모리 구조

Java 프로그램이 시작되면 JVM이 운영체제에서 일부 메모리를 가져온다. 

JVM은 모든 요구사항에 이 메모리를 사용하고, 데이터를 **데이터 타입(자료형)**에 따라서 해당 공간에 할당한다.



**메모리 공간 구분 :**

- Static 영역
- Stack 영역
- Heap 영역

- ~~기계어 관련 영역~~ 



**Java파일의 구성 :**

- 필드(field)
- 생성자(constructor)
- 메소드(method)

<br>

<br>

### Static area(스태틱 메모리 영역)

**필드 부분에서 선언된 변수(전역 변수)와 정적 멤버 변수(static이 붙은 자료형)은 Static 영역에 데이터를 저장한다.**

👉Static 영역의 데이터는 **프로그램의 시작부터 종료까지** 메모리에 계속 남아있는다.(무분별한  static 활용으로 메모리 부족에 주의.)

이것이 전역 변수가 프로그램이 종료될 때까지 어디서든 사용이 가능한 이유.

<br>

<br>

### Stack area(스택 메모리 영역)

**메소드 내에서 정의하는 기본 자료형**(int, double, byte, long, boolean 등)에 해당되는 **지역변수**(매개 변수 및 블럭문 내 변수 포함)의 데이터 값이 저장되는 공간.

👉해당 메소드가 **호출될 때 메모리에 할당**되고, **종료되면 메모리가 해제**된다.



```java
public class StackAreaEx {
	public static void main(String[] args) {
		int a = 5;	a = 4;	a = 3;	a = 2;
		System.out.println(a);
		for(int i=0; i<5; i++){
		}
//		System.out.println(i); 컴파일 에러
	}
}
```

int a = 5, a = 4.... 이렇게 입력된 변수의 값은 결국 마지막에 입력된 값만을 가진다(이전의 데이터는 지워진다).

👉**Stack(Last In First Out) 영역에 할당된 지역변수** 이기 때문에.

👉**for문 안의 i 또한 지역변수** 이므로, **for문 종료와 동시에 i는 Stack 영역에서 해제**되는 것이다.

<br>

<br>

### Heap area(힙 메모리 영역)

**참조형(Reference Type)의 데이터 타입을 갖는 객체(인스턴스), 배열 등은 Heap 영역에 데이터가 저장**된다. 

이때 **변수**(객체, 객체변수, 참조변수)는 **Stack 영역의 공간에서 실제 데이터가 저장된 Heap 영역의** 참조값(reference value, 해시코드 / 메모리에 저장된 주소를 연결해주는 값)**을** **new 연산자를 통해 리턴** 받는다.

👉**실제 데이터를 갖고 있는 객체를 통해서만 해당 인스턴스를 다룰 수 있다.**



```java
public class HeapAreaEx01 {
	public static void main(String[] args) {
		int[] a = null; // int형 배열 선언 및 Stack 영역 공간 할당
		System.out.println(a); // 결과 : null
		a = new int[5]; // Heap 영역에 5개의 연속된 공간 할당 및 
		                // 변수 a에 참조값 할당
		System.out.println(a); // 결과 : @15db9742 (참조값)
	}
}
```

결국 변수 a는 데이터가 저장된 Heap 영역의 참조값을 리턴 받아 갖고 있다는 것.



```java
public class HeapAreaEx02 {
	public static void main(String[] args) {
		String str1 = new String("joker");
		String str2 = new String("joker");
		if(str1 == str2){
			System.out.println("같은 주소값 입니다.");
		}else{
			System.out.println("다른 주소값 입니다.");
		}
	}
}
```

위 코드의 결과는 "다른 주소값 입니다." 이다.

 👉**new 연산자를 이용해서 생성하면 데이터는 힙 메모리 영역에 새로(new) 저장**되고,

str1과 str2는 각각의 참조 값을 리턴받는다. 두 변수에 저장된 주소가 다르기 때문에, "==" 로 비교했을 때 false가 나오게 된다.



```java
class A{}

public class HeapArea {
	public static void main(String[] args) {
		A a = null; // A타입의 a객체 선언 및 Stack 영역 공간 할당
		System.out.println(a); // 결과 : null
		a = new A(); // Heap 메모리에 공간 할당 및 객체(a)에 참조값 할당
		System.out.println(a); // 결과 : @15db9742
	}
}
```

✔결국 객체가 참조 값을 가진다는 것.

![image-20220707145956056](../images/2022-07-07-(JAVA) 메모리 구조(static, stack, heap)/image-20220707145956056.png)

<br>

<br>

------

+α

Heap에 저장된 데이터가 더이상 사용이 불필요하다면 메모리 관리를 위해 JVM에 의해 알아서 해제된다.

이러한 기능을 가바지컬렉션(GC) 라고 한다.

<br>

객체 관련 추가 설명 : [Java 클래스와 객체, 그리고 인스턴스(dodokyumin 깃헙 블로그)](https://dodokyumin.github.io/Java-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80-%EA%B0%9D%EC%B2%B4,-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4/)

<br>

<br>

<br>

## References

> [JOKER's ROOM(네이버 블로그)](https://m.blog.naver.com/heartflow89/220954420688)
