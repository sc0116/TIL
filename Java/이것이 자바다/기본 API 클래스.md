# 7. String 클래스
- 자바의 문자열은 java.lang 패키지의 String 클래스의 인스턴스로 관리됩니다.

## String 메소드
|리턴 타입|메소드명(매개 변수)|설명
|:---|:---|:---|
|char|charAt(int index)|특정 위치의 문자 리턴|
|boolean|equals(Object anObject)|두 문자열을 비교|
|byte[]|getBytes()|byte[]로 리턴|
|byte[]|getBytes(Charset charset)|주어진 문자셋으로 인코딩한 byte[]로 리턴|
|int|indexOf(String str)|문자열 내에서 주어진 문자열의 위치를 리턴|
|int|length()|총 문자의 수를 리턴|
|String|replace(CharSequence target, CharSequence replacement)|target 부분을 replacement로 대치한 새로운 문자열을 리턴|
|String|substring(int beginIndex)|beginIndex 위치에서 끝까지 자라낸 새로운 문자열을 리턴|
|String|substring(int beginIndex, int endIndex)|beginIndex 위치에서 endIndex 전까지 잘라낸 새로운 문자열을 리턴|
|String|toLowerCase()|알파벳 소문자로 변환한 새로운 문자열을 리턴|
|String|toUpperCase()|알파벳 대문자로 변환한 새로운 문자열을 리턴|
|String|trim()|앞뒤 공백을 제거한 새로운 문자열을 리턴|
|String|valueOf(int i)  valueOf(double d)|기본 타입값을 문자열로 리턴|

- ### 문자 추출(charAt())
    - charAt() 메소드는 **매개값으로 주어진 인덱스**의 문자를 리턴합니다.
```java
String subject = "자바 프로그래밍";
char charValue = subject.charAt(3); //'프'
```
- ### 문자열 비교(equals())
    - 문자열을 비교할 때에는 == 연산자를 사용하면 원하지 않는 결과가 나올 수 있습니다.
    - 두 String 객체의 문자열만을 비교하고 싶다면 equals() 메소드를 사용해야 합니다.
```java
public class StringEqualsExample {
    public static void main(String[] args) {
        String strVar1 = new String("신승철");
        String strVar2 = "신승철";
        
        if (strVar1 == strVar2) {
            System.out.println("같은 String 객체를 참조");
        } else {
            System.out.println("다른 String 객체를 참조");
        }
        
        if (strVar1.equals(strVar2)) {
            System.out.println("같은 문자열을 가짐");
        } else {
            System.out.println("다른 문자열을 가짐");
        }
        
        //출력 결과
        //다른 String 객체를 참조
        //같은 문자열을 가짐
    }
}
```
- ### 바이트 배열로 변환(getBytes())
    - getBytes() 메소드는 시스템의 기본 문자셋으로 인코딩된 바이트 배열을 리턴합니다.
```java
byte[] bytes = "문자열".getBytes();
byte[] bytes = "문자열".getBytes(Charset charset);
```
- ### 문자열 찾기(indexOf())
    - indexOf() 메소드는 매개값으로 주어진 문자열이 시작되는 인덱스를 리턴합니다.
    - 만약 주어진 문자열이 포함되어 있지 않으면 -1을 리턴합니다.
```java
String subject = "자바 프로그래밍";
int index = subject.indexOf("프로그래밍");   //3(찾는 문자열의 시작 인덱스 위치)
```
- ### 문자열 길이(length())
    - length() 메소드는 문자열의 길이를 리턴합니다.
```java
String subject = "자바 프로그래밍";
int length = subject.length();  //8
```

- ### 문자열 대치(replace())
    - replace() 메소드는 첫 번째 매개값인 문자열을 찾아 두 번째 매개값인 문자열로 대치한 새로운 문자열을 생성하고 리턴합니다.
    - String 객체의 문자열은 변경이 불가한 특성을 갖기 때문에 replace() 메소드가 리턴하는 문자열은 원래 문자열의 수정본이 아니라 완전히 새로운 문자열입니다.
```java
String oldStr = "자바 프로그래밍";
String newStr = oldStr.replace("자바", "JAVA");   //"JAVA 프로그래밍"
```
- ### 문자열 잘라내기(substring())
    - substring() 메소드는 주어진 인덱스에서 문자열을 추출합니다.
```java
String str = "이것은 자바일까 바자일까";
String firstNum = str.substring(0, 8);  //"이것은 자바일까"(인덱스 0 포함 ~ 8 제외)
String secondNum = str.substring(9);    //"바자일까"(9 이후의 문자열 추출)
```
- ### 알파벳 대소문자 변경(toLowerCase(), toUpperCase())
    - toLowerCase() 메소드는 문자열을 모두 소문자로 바꾼 새로운 문자열을 생성한 후 리턴합니다.
    - toUpperCase() 메소드는 문자열을 모두 대문자로 바꾼 새로운 문자열을 생성한 후 리턴합니다.
```java
String original = "Java Programming";
String lowerCase = original.toLowerCase(original);  //"java programming"
String upperCase = original.toUpperCase(original);  //"JAVA PROGRAMMING"
```
- ### 문자열 앞뒤 공백 잘라내기(trim())
    - trim() 메소드는 문자열의 앞뒤 공백을 제거한 새로운 문자열을 생성하고 리턴합니다.
    - 앞뒤의 공백만 제거할 뿐 중간의 공백은 제거하지 않습니다.
```java
String oldStr = "    자바 프로그래밍    ";
String newStr = oldStr.trim();  //"자바 프로그래밍"
```
- ### 문자열 변환(valueOf())
    - valueOf() 메소드는 기본 타입의 값을 문자열로 변환하는 기능을 가지고 있습니다.
```java
int i = 10;                     //10
String str = String.valueOf(i); //"10"
```