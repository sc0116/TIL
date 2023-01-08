# 문자열 생성 방법
객체를 생성할 때는 new 연산자를 사용하여 객체를 생성한다. 
그러나 문자열은 new 연산자를 이용한 생성 방법뿐만 아니라 문자열 리터럴을 생성하는 방법도 있다.

```java
String str1 = "짱구";                 // 문자열 리터럴 생성
String str2 = new String("짱구");     // new 연산자를 이용한 문자열 생성
```

두 방법의 차이점은 실제 메모리에 할당되는 영역에 있다.

## 문자열 리터럴 생성
문자열 리터럴을 이용하여 생성하는 경우 **String Constant Pool이라는 영역에 할당**된다. 
String Constant Pool 영역은 Java Heap Memory 내에 위치해 있으며, 
만약 String Constant Pool에 값이 이미 있다면 그 참조값을 반환하고, 없다면 String Constant Pool에 문자열을 등록한 후 해당 참조값이 반환되는 형태이다. 

```java
String str1 = "짱구";
String str2 = "짱구";
```

위 코드를 실행시키면 문자열이 저장되는 메모리의 모습은 아래와 같다.

![문자열 리터럴](https://user-images.githubusercontent.com/47477359/211199796-9cdbf663-c1d9-4d35-a92c-899817078271.png)

문자열 리터럴을 이용하여 생성한 객체인 str1과 str2는 String Constant Pool 내의 동일한 객체를 바라본다.

## new 연산자를 이용한 문자열 생성
new 연산자를 이용하여 문자열을 생성하는 경우 **Heap 영역에 할당**된다.

```java
String str1 = "짱구";
String str2 = "짱구";
String str3 = new String("짱구");
String str4 = new String("짱구");
```

위 코드를 실행시키면 문자열이 저장되는 모습은 아래와 같다.

![new 연산자](https://user-images.githubusercontent.com/47477359/211199793-236a440c-c255-496d-a759-06da595f116b.png)

new 연산자로 생성한 객체인 str3과 str4는 Heap에 서로 다른 객체를 바라본다.
