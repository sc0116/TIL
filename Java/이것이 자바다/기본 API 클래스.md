# 1. 자바 API 도큐먼트
- API는 라이브러리라고 부르기도 하는데, 프로그램 개발에 **자주 사용되는 클래스 및 인터페이스의 모음**을 말합니다.
- [API 도큐먼트](https://docs.oracle.com/javase/8/docs/api) 는 쉽게 API를 찾아 이용할 수 있도록 문서화한 것을 말합니다. 

# 2. java.lang과 java.util 패키지
- 자바 애플리케이션을 개발할 때 공통적으로 가장 많이 사용하는 패키지는 java.lang 패키지와 java.util, java.time 패키지일 것입니다.

## java.lang 패키지
- java.lang 패키지는 **자바 프로그램의 기본적인 클래스**를 담고 있는 패키지입니다.
- 그렇기 때문에 java.lang 패키지에 있는 클래스와 인터페이스는 **import 없이 사용**할 수 있습니다.
<table>
<tr><th colspan="2">클래스</th><th>용도</th></tr>
<tr><td colspan="2">Object</td><td>- 자바 클래스의 최상위 클래스로 사용</td></tr>
<tr><td colspan="2">System</td><td>- 표준 입력 장치(키보드)로부터 데이터를 입력받을 때 사용<br>- 표준 출력 장치(모니터)로 출력하기 위해 사용<br>- 자바 가상 기계를 종료시킬 때 사용<br>- 쓰레기 수집기를 실행 요청할 때 사용</td></tr>
<tr><td colspan="2">Class</td><td>- 클래스를 메모리로 로딩할 때 사용</td></tr>
<tr><td colspan="2">String</td><td>- 문자열을 저장하고 여러 가지 정보를 얻을 때 사용</td></tr>
<tr><td colspan="2">StringBuffer, StringBuilder</td><td>- 문자열을 저장하고 내부 문자열을 조작할 때 사용</td></tr>
<tr><td colspan="2">Math</td><td>- 수학 함수를 이용할 때 사용</td></tr>
<tr><td>Wrapper</td><td>Byte, Short, Character, Integer, Float, Double, Boolean, Long</td><td>- 기본 타입의 데이터를 갖는 객체를 만들 때 사용<br>- 문자열을 기본 타입으로 변환할 때 사용<br>- 입력값 검사에 사용</td></tr>
</table>

## java.util 패키지
- java.util 패키지는 컬렉션 클래스들이 대부분을 차지하고 있습니다.

클래스|용도
:---|:---
Arrays|- 배열을 조작(비교, 복사, 정렬, 찾기)할 때 사용
Calendar|- 운영체제의 날짜와 시간을 얻을 때 사용
Date|- 날짜와 시간 정보를 저장하는 클래스
Objects|- 객체 비교, 널(null) 여부 등을 조사할 때 사용
StringTokenizer|- 특정 문자로 구분된 문자열을 뽑아낼 때 사용
Random|- 난수를 얻을 때 사용

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

# 8. StringTokenizer 클래스
- 문자열이 **특정 구분자(delimiter)로 연결**되어 있을 경우, 
  **구분자를 기준으로 부분 문자열을 분리**하기 위해서는 String의 split() 메소드를 이용하거나, 
  java.util 패키지의 StringTokenizer 클래스를 이용할 수 있습니다.  
- split()은 **정규 표현식**으로 구분하고, StringTokenizer는 **문자**로 구분한다는 차이점이 있습니다.

## split() 메소드
- **정규 표현식**을 구분자로 해서 문자열을 분리한 후, 배열에 저장하고 리턴합니다.
```java
String[] result = "문자열".split("정규표현식");

//예시
String text = "홍길동&이수홍,박연수,김자바-최명호";
String[] names = text.split("& | , | -");   //["홍길동", "이수홍", "박연수", "김자바", "최명호"]
```

## Stringtokenizer 클래스
- 문자열이 **한 종류의 구분자**로 연결되어 있을 경우, StringTokenizer 클래스를 사용하면 쉽게 문자열을 분리해 낼 수 있습니다.
- StringTokenizer 객체를 생성할 때 첫 번재 매개값으로 전체 문자열을 주고 두 번째 매개값으로 구분자를 주면 됩니다.
```java
StringTokenizer st = new StringTokenizer("문자열", "구분자");

//예시
String text = "홍길동/이수홍/박연수/김자바";
StringTokenizer st = new StringTokenizer(text, "/");    ["홍길동", "이수홍", "박연수", "김자바"]
```
- 객체가 생성되면 다음 메소드들을 이용해서 부분 문자열을 분리해 낼 수 있습니다.  

|리턴 타입|메소드명|설명|
|:---|:---|:---| 
|int|countTokens()|꺼내지 않고 남아 있는 토큰의 수|
|boolean|hasMoreTokens()|남아 있는 토큰이 있는지 여부|
String|nextToken()|토큰을 하나씩 꺼내옴|
```java
String text = "신/승/철";

StringTokenizer st = new StringTokenizer(text, "/");
while (st.hasMoreTokens()) {
    System.out.println(st.nextToken());
}
```

# 9. StringBuffer, StringBuilder 클래스
- 문자열을 저장하는 String은 내부의 **문자열을 수정할 수 없습니다.**
- String의 replace() 메소드는 내부의 문자를 대치하는 것이 아니라, 대치된 **새로운 문자열을 리턴**합니다.
- 문자열을 결합하는 + 연산자를 많이 사용하면 할수록 그만큼 String **객체의 수**가 늘어나기 때문에,
  **프로그램 성능을 느리게 하는 요인**이 됩니다.
- **문자열을 변경**하는 작업이 많을 경우에는 java.util 패키지의 StringBuffer 또는 StringBuilder 클래스를 사용하는 것이 좋습니다.

## StringBuffer와 StringBuilder의 차이점
- 사용 방법은 동일하지만, StringBuffer는 **멀티 스레드 환경**에서 사용할 수 있도록 **동기화가 적용**되어 있어 **스레드에 안전**합니다.
- 반면, StringBuilder는 **단일 스레트 환경**에서만 사용하도록 설계되어 있습니다.
```java
StringBuilder sb = new StringBuilder();
StringBuffer sb = new StringBuffer();
```

## StringBuilder 메소드
메소드|설명
:---|:---
append(매개값)|문자열 끝에 주어진 매개값을 추가
insert(int offset, 매개값)|문자열 중간에 주어진 매개값을 추가
delete(int start, int end)|문자열의 일부분을 삭제
deleteCharAt(int index)|문자열에서 주어진 index의 문자를 삭제
replace(int start, int end, String str)|문자열의 일부분을 다른 문자열로 대치
StringBuilder reverse()|문자열의 순서를 뒤바꿈
setCharAt(int index, char ch)|문자열에서 주어진 index의 문자를 다른 문자로 대치
```java
StringBuilder sb = new StringBuilder();

sb.append("Java ").append("Program Study");  //"Java Program Study"

sb.insert(4, '2');          //Java2 Program Study (4번째 문자 뒤에 2를 삽입)
sb.setCharAt(4, '6');       //Java6 Program Study (4번쨰 문자 뒤의 문자를 6으로 변경)
sb.replace(6, 13, "Book");  //Java6 Book Study (6번째 문자 뒤부터 13번째 문자까지를 "Book" 문자열로 대치
sb.delete(4, 5);            //Java Book Study (5번째 문자를 삭제)
```