```
layout: single
title: "Java 내장 객체 (Webmarket 구현 3)" #글 제목
categories: Java #글이 담길 폴더(대목차)
tag: [Bean, Jsp] #태그 넣어주기
```

# 내장 객체

**순서**

1. 모델객체 만들고

2. 기능제공하는 레파지토리 만들고

3. **화면에 뿌린다. (jsp)**

   <details open="" style="box-sizing: border-box; margin-top: 0px; margin-bottom: 0px;"><summary style="box-sizing: border-box; font-weight: bold; font-size: 15px; color: rgb(228, 57, 20);">실습 목표</summary><div style="box-sizing: border-box; padding-top: 0px;"><img src="../images/2022-06-13-Java 내장 객체 (Webmarket 구현 3)/1.png" data-src="../images/2022-06-13-Java 내장 객체 (Webmarket 구현-3)/image-20220613180304121.png" style="box-sizing: border-box; border-width: 0px 4px 0px 2px; border-top-style: initial; border-right-style: solid; border-bottom-style: initial; border-left-style: solid; border-top-color: initial; border-right-color: transparent; border-bottom-color: initial; border-left-color: transparent; border-image: initial; vertical-align: middle; max-width: 100%; image-orientation: from-image; cursor: default;"><span>&nbsp;</span><img src="../images/2022-06-13-Java 내장 객체 (Webmarket 구현 3)/4.png" data-src="../images/2022-06-13-Java 내장 객체 (Webmarket 구현-3)/image-20220613180320544.png" style="box-sizing: border-box; border-width: 0px 4px 0px 2px; border-top-style: initial; border-right-style: solid; border-bottom-style: initial; border-left-style: solid; border-top-color: initial; border-right-color: transparent; border-bottom-color: initial; border-left-color: transparent; border-image: initial; vertical-align: middle; max-width: 100%; image-orientation: from-image; cursor: default;"></div></details>

## 3. 화면에 뿌리기 위한 Jsp 만들기

jsp 파일은 일반적으로 webapp 폴더에 넣는다.

![3](../images/2022-06-13-Java 내장 객체 (Webmarket 구현 3)/3.png)

### 상품 목록 화면 만들기

#### 자바 빈(bean) (products.JSP)사용하기

```jsp
<jsp:useBean id="repository"
    class="com.webmarket.data.ProductRepository" scope="session"></jsp:useBean>
```

이 빈(bean) 코드는 결국

```java
ProductRepository repository = new ProductRepository;
```

이렇게 new 해준 것과 같은 의미이다.

**<ProductsRepository.java 파일에서 getAllProducts 메소드 구현>**

```java
public List<Product> getAllProducts() {
    return products;
}
```

**<JSP 파일에서 Bootstrap을 활용하여 화면 출력하기>**

```html
<div class="contianer">
            <div class="row" align="center">
                <%
                    List<Product> products = repository.getAllProducts();
                    for(int i = 0; i < products.size(); i++){
                        Product product = products.get(i);
                %>
                <div class="col-md-4">
                    <h3><%= product.getName() %></h3>
                    <p><%= product.getDescription() %></p>
                    <p><%= product.getUnitPrice() %> 원 </p>
                    <p><a href="product.jsp?id=<%= product.getId() %>" class = "btn btn-secondary">상세 정보 &raquo;</a></p>
                </div>
                <%
                }
                %>
            </div>
        </div>
```

받은 값을 바로 출력해주는 기능의 "**<%= %>**"

**<출력 화면>**

![1](../images/2022-06-13-Java 내장 객체 (Webmarket 구현 3)/1.png)

### 상품 상세 화면 만들기

**<ProductsRepository.java 파일에서 getProductsById 메소드 구현>**

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

**<출력 화면>**

![4](../images/2022-06-13-Java 내장 객체 (Webmarket 구현 3)/4.png)
