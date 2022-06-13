# 내장 객체



**순서** 

1. 모델객체 만들고

2. 기능제공하는 레파지토리 만들고

3. **화면에 뿌린다. (jsp)**

   

   <details>
     <summary style="font-Weight : bold; font-size : 15px; color : #E43914;" >실습 목표</summary>
     <div>
       <img src="../images/2022-06-13-Java 클래스를 정의했을 때 할 일/image-20220613093044400.png"/>
       <img src="../images/2022-06-13-Java 내장 객체 (Webmarket 구현-3)/image-20220613145600602.png"/>
     </div>
   </details>
   
   
   

------



.stream();

list를 Stream으로 데이터의 흐름.

흐름을 타고 product들을 가져오는데 filter를 통해 거른다.

```java
    public Product getProductById(String id) {
    	return products.stream()
    			.filter((product) -> product.getId().equals(id))//filter로 조건 걸기
    			.findFirst() //첫번째
    			.get(); //얻어
    }
```

