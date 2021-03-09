# 2021.03.09 화요일

```java
private int getBoardCntByMemberNo(final long memberNo, PlatformType platformType)
```

위와 같은 메소드가 정의되어 있을 때, 첫 번째 parameter 앞에 붙은 final은 어떤 의미일까?

- 메소드의 호출부 쪽에서 쓰인 memberNo값과 다르지 않음을 의미한다 (X)  
   : java는 method에 parameter 값(value)을 copy해서 전달한다.
  즉, method로 전달된 parameter는 원본과 다른 영역에 할당된 변수로서, 원본과 같은 값을 가진다.

- parameter로 받은 값이 메소드 내부에서 변경될 수 없음을 의미한다 (O)  
   : final 키워드가 붙은 변수는 값이 할당된 이후 변경할 수 없다.
  따라서, pass-by-value로 전달된 변수이기에 값을 재할당 할 수 없다.

- parameter가 Object인 경우라면?  
  : 객체의 pointer(value)를 copy해서 전달받았다고 생각하면 된다.
  (Java는 항상 pass-by-value임을 명심하자. Java에서의 객체는 실제로 객체의 주소 값(value)이다. parameter로 전달될 때는 이러한 pointer 값이 전달된다.)
  이러한 pointer 값에 final 키워드가 붙는 것이므로, 객체의 주소값을 재할당할 수 없고 객체에 접근하여 속성을 변경할 수는 있다.

> 요약: 파라미터의 값을 메소드 내부에서 변경하지 못하도록, 명시적으로 parameter에 final 키워드를 붙인다.
