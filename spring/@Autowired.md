# 2021.03.09 화요일

## @Autowired
Spring 내부에서의 작동 방법

## 활용

```java
Class myClass {
  @Autowired
  List<MyInterface> myInterfaceList;
}
```

MyInterface를 구현하는 모든 Bean을 List에 담아 myClass의 멤버변수인 myInterfaceList에 주입한다.

## 추가
IntelliJ에서 이 어노테이션을 활용한 필드 주입을 시도하면, 아래와 같은 문구가 뜬다.
 ```
대충 필드 주입을 권장하지 않으니 생성자를 정의하여 주입하라는 뜻
```
