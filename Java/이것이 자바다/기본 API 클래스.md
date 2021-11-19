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

# 3. Object 클래스
- Object는 자바의 최상위 부모 클래스에 해당합니다.
- 클래스를 선언할 때 extends 키워드로 다른 클래스를 상속하지 않으면 암시적으로 java.lang.Object 클래스를 상속하게 됩니다.

## 객체 비교(equals())
```java
public boolean equals(Object obj) { ... }
```
- equals() 메소드의 매개 타입은 Object인데, 이것은 모든 객체가 매개값으로 대입될 수 있음을 말합니다.
- 그 이유는 Object가 최상위 타입이므로 모든 객체는 Object 타입으로 자동 타입 변환될 수 있기 때문입니다.
- equals() 메소드는 두 객체를 비교해서 **논리적으로 동등하면 true를 리턴**하고, 그렇지 않으면 false를 리턴합니다.
- 논리적으로 동등하다는 것은 같은 객체이건 다른 객체이건 상관없이 **객체가 저장하고 있는 데이터가 동일함**을 뜻합니다.
- equals() 메소드를 재정의할 때에는 매개값(비교 객체)이 기준 객체와 동일한 타입의 객체인지 instanceof 연산자로 먼저 확인해야 합니다.
```java
public class Member {
    public String id;
    
    public Member(String id) {
        this.id = id;
    }
    
    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Member) {
            Member member = (Member) obj;
            if (id.equals(member.id)) {
                return true;
            }
        }
        return false;
    }
}

public class MemberExample {
  public static void main(String[] args) {
    Member obj1 = new Member("blue");
    Member obj2 = new Member("blue");
    Member obj3 = new Member("red");

    if (obj1.equals(obj2)) {
      System.out.println("obj1과 obj2는 동등합니다.");
    } else {
      System.out.println("obj1과 obj2는 동등하지 않습니다.");
    }
    
    if (obj1.equals(obj3)) {
      System.out.println("obj1과 obj3는 동등합니다.");
    } else {
      System.out.println("obj1과 obj3는 동등하지 않습니다.");
    }
  }
}
```

## 객체 해시코드(hashCode())
- 객체 해시코드란 객체를 식별할 하나의 정수값을 말합니다.
- hashCode() 메소드는 객체의 메모리 번지를 이용해서 해시코드를 만들어 리턴하기 때문에 객체마다 다른 값을 가지고 있습니다.
- 객체의 동등 비교를 위해서는 Object의 equals() 메소드만 재정의하지 말고 hashCode() 메소드도 재정의해서 논리적 동등 객체일 경우 동일한 해시코드가 리턴되도록 해야 합니다.
```java
public class Member {
    public String id;
    
    public Member(String id) {
        this.id = id;
    }
    
    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Member) {
            Member member = (Member) obj;
          if (id.equals(member.id)) {
              return true;
          }
        }
        return false;
    }
    
    @Override
    public int hashCode() {
        return id.hashCode();
    }
}
```

## 객체 문자 정보(toString())
- toString() 메소드는 "클래스명@16진수해시코드"로 구성된 객체의 문자 정보를 리턴합니다.
- 자바 애플리케이션에서는 별 값어치가 없는 정보이므로 Object 하위 클래스는 toString() 메소드를 재정의하여 간결하고 유익한 정보를 리턴하도록 되어 있습니다.
- 우리가 만드는 클래스도 toString() 메소드를 재정의해서 좀 더 유용한 정보를 리턴하도록 할 수 있습니다.
```java
public class SmartPhone {
    private String company;
    private String os;
    
    public SmartPhone(String company, String os) {
        this.company = company;
        this.os = os;
    }
    
    @Override
    public String toString() {
        return company + ", " + os;
    }
}

public class SmartPhoneExample {
  public static void main(String[] args) {
    SmartPhone myPhone = new SmartPhone("구글", "안드로이드");
    
    Stirng strObj = myPhone.toString();
    System.out.println(strObj);
    
    //myPhone.toString()을 자동 호출해서 리턴값을 얻은 후 출력
    System.out.println(myPhone);
  }
}
```
- System.out.println() 메소드는 매개값이 기본 타입(byte, short, int, long, float, double, boolean)일 경우, 해당 값을 그대로 출력합니다.
- 만약 매개값이 객체일 경우, toString() 메소드를 호출해서 리턴값을 받아 출력하도록 되어 있습니다.

## 객체 복제(clone())
- 객체 복제는 원본 객체의 필드값과 동일한 값을 가지는 새로운 객체를 생성하는 것을 말합니다.
- 객체를 복제하는 이유는 원본 객체를 안전하게 보호하기 위해서입니다.

### 얕은 복제(thin clone)
- 얕은 복제란 단순히 필드값을 복사해서 객체를 복제하는 것을 말합니다.
- 필드값만 복제하기 때문에 필드가 기본 타입일 경우 값 복사가 일어나고, 필드가 참조 타입일 경우에는 객체의 번지가 복사됩니다.
- Object의 clone() 메소드는 자신과 동일한 필드값을 가진 얕은 복제된 객체를 리턴합니다.
- 이 메소드로 객체를 복제하려면 원본 객체는 반드시 java.lang.Cloneable 인터페이스를 구현하고 있어야 합니다.
- 메소드 선언이 없음에도 불구하고 Cloneable 인터페이스를 명시적으로 구현하는 이유는 **클래스 설계자가 복제를 허용한다는 의도적인 표시**를 하기 위해서입니다.
- Cloneable 인터페이스를 구현하지 않으면 clone() 메소드를 호출할 때 CloneNotSupportedException 예외가 발생하여 복제가 실패됩니다.
```java
public class Member implements Cloneable {
    public String id;
    public String name;
    public String password;
    
    public Member(String id, String name, String password) {
        this.id = id;
        this.name = name;
        this.password = password;
    }
    
    public Member getMember() {
      Member cloned = null;

      try {
        cloned = (Member) clone();
      } catch (CloneNotSupportedException e) { }
      return cloned;
    }
}

public class MemberExample {
  public static void main(String[] args) {
    //원본 객체 생성
    Member original = new Member("blue", "홍길동", "12345");
    
    //복제 객체를 얻은 후에 패스워드 변경
    Member cloned = original.getMember();
    cloned.password = "67890";

    System.out.println("[복제 객체의 필드값]");
    System.out.println("id: " + cloned.id);
    System.out.println("name: " + cloned.name);
    System.out.println("password: " + cloned.password);

    System.out.println("[원본 객체의 필드값]");
    System.out.println("id: " + original.id);
    System.out.println("name: " + original.name);
    
    //원본 객체의 패스워드는 변함 없음
    System.out.println("password: " + original.password);
  }
}
```

### 깊은 복제(deep clone)
- 얕은 복제의 경우 참조 타입 필드는 번지만 복제되기 때문에 원본 객체의 필드와 복제 객체의 필드는 같은 객체를 참조하게 됩니다.
- 만약 복제 객체에서 참조 객체를 변경하면 원본 객체도 변경된 객체를 가지게 됩니다.
- 깊은 복제란 참조하고 있는 객체도 복제하는 것을 말합니다.
- 깊은 복제를 하려면 Object의 clone() 메소드를 재정의해서 참조 객체를 복제하는 코드를 직접 작성해야 합니다.
```java
public class Member implements Cloneable {
    public String name;
    public int[] scores;
    public Car car;
    
    public Member(String name, int[] scores, Car car) {
        this.name = name;
        this.scores = scores;
        this.car = car;
    }
    
    @Override
    protected Object clone() throws CloneNotSupportedException {
        //먼저 얕은 복사를 해서 name을 복제한다.
        Member cloned = (Member) super.clone();
        //scores를 깊은 복제한다.
        cloned.scores = Arrays.copyOf(this.scores, this.scores.length);
        //car를 깊은 복제한다.
        cloned.car = new Car(this.car.model);
        //깊은 복제된 Member 객체를 리턴
        return cloned;
    }
    
    public Member getMember() {
        Member cloned = null;
        try {
            cloned = (Member) clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return cloned;
    }
}

public class Car {
    public String model;
    
    public Car(String model) {
        this.model = model;
    }
}

public class MemberExample {
  public static void main(String[] args) {
    //원본 객체 생성
    Member original = new Member("홍길동", new int[] {90, 90}, new Car("소나타"));
    
    //복제 객체를 얻은 후에 참조 객체의 값을 변경
    Member cloned = original.getMember();
    cloned.scores[0] = 100;
    cloned.car.model = "그랜저";

    System.out.println("[복제 객체의 필드값]");
    System.out.println("name: " + cloned.name);
    System.out.println("scores: " + Arrays.toString(cloned.scores));
    System.out.println("car: " + cloned.car.model);

    System.out.println("[원본 객체의 필드값]");
    System.out.println("name: " + original.name);
    System.out.println("scores: " + Arrays.toString(original.scores));
    System.out.println("car: " + original.car.model);
  }
}
```

## 객체 소멸자(finalize())
- 참조하지 않는 배열이나 객체는 쓰레기 수집기 (Garbage Collector)가 힙 영역에서 자동적으로 소멸시킵니다.
- 쓰레기 수집기는 객체를 소멸하기 직전에 마지막으로 객체의 소멸자를 실행시킵니다.
- 소멸자는 Object의 finalize() 메소드를 말하는데, 기본적으로 실행 내용이 없습니다.
- 만약 객체가 소멸되기 전에 마지막으로 사용했던 자원을 닫고 싶거나, 중요한 데이터를 저장하고 싶다면 finalize()를 재정의할 수 있습니다.
```java
public class Counter {
    private int no;
    
    public Counter(int no) {
        this.no = no;
    }
    
    @Override
    protected void finalize() throws Throwable {
      System.out.println(no + "번 객체의 finalize()가 실행됨");
    }
}

public class FinalizeExample {
  public static void main(String[] args) {
    Counter counter = null;
    for (int i = 1; i <= 50; i++) {
        counter = new Counter();
        
        //Counter 객체를 쓰레기로 만듬
        counter = null;
        
        //쓰레기 수집기 실행 요청
        System.gc();
    }
  }
}
```
- 실행 결과를 보면 순서대로 소멸시키지 않고, 무작위로 소멸시키는 것을 볼 수 있습니다.
- 그리고 전부 소멸시키는 것이 아니라 메모리의 상태를 보고 일부만 소멸시킵니다.
- 예제에서는 System.gc()로 쓰레기 수집기를 실행 요청하였으나, 쓰레기 수집기는 메모리가 부족할 때 그리고 CPU가 한가할 때에 JVM에 의해서 자동 실행됩니다.

# 4. Objects 클래스
- java.util.Objects 클래스는 객체 비교, 해시코드 생성, null 여부, 객체 문자열 리턴 등의 연산을 수행하는 정적 메소드들로 구성된 Object의 유틸리티 클래스입니다.

리턴 타입|메소드(매개 변수)|설명
:---|:---|:---
int|compare(T a, T b, Comparator<T> c)|두 객체 a와 b를 Comparator를 사용해서 비교
boolean|deepEquals(Object a, Object b)|두 객체의 깊은 비교(배열의 항목까지 비교)
boolean|equals(Object a, Object b)|두 객체의 얕은 비교(번지만 비교)
int|hash(Object... values)|매개값이 저장된 배열의 해시코드 생성
int|hashCode(Object o)|객체의 해시코드 생성
boolean|isNull(Object obj)|객체가 null인지 조사
boolean|nonNull(Object obj)|객체가 null이 아닌지 조사
T|requireNonNull(T obj)|객체가 null인 경우 예외 발생
T|requireNonNull(T obj, String message)|객체가 null인 경우 예외 발생(주어진 예외 메시지 포함)
T|requireNonNUll(T obj, Supplier<String> messageSupplier)|객체가 null인 경우 예외 발생(람다식이 만든 예외 메시지 포함)
String|toString(Object o)|객체의 toString() 리턴값 리턴
String|toString(Object o, String nullDefault)|객체의 toString() 리턴값 리턴, 첫 번째 매개값이 null일 경우 두 번째 매개값 리턴

## 객체 비교(compare(T a, T b, Comparator<T> c))
- 두 객체를 비교자(Comparator)로 비교해서 int 값을 리턴합니다.
- a가 b보다 작으면 음수, 같으면 0, 크면 양수를 리턴하도록 구현 클래스를 만들어야 합니다.
```java
public class CompareExample { 
    public static void main(String[] args) {
        Student s1 = new Student(1);
        Student s2 = new Student(1);
        Student s3 = new Student(2);
        
        int result = Objects.compare(s1, s2, new StudentComparator());
        System.out.println(result);
        result = Objects.compare(s1, s3, new StudentComparator());
        System.out.println(result);
    }

    static class Student {
        itn sno;
        Student(int sno) {
            this.sno = sno;
        }
    }
    
    static class StudentComparator implements Comparator<Student> {
        @Override
        public int compare(Student o1, Student o2) {
            /*
            if (o1.sno < o2.sno) return -1;
            else if (o1.sno == o2.sno) return 0;
            else return 1;
            */
            return Integer.compare(o1.sno, o2.sno);
        }
    }
}
```

## 동등 비교(equals()와 deepEquals())

### equals()
- Objects.equals(Object a, Object b)는 두 객체의 동등을 비교하는데 다음과 같은 결과를 리턴합니다.
- 특이한 점은 a와 b가 모두 null일 경우 true를 리턴한다는 점입니다.

a|b|Objects.equals(a, b)
:---|:---|:---
not null|not null|a.equals(b)의 리턴값
null|not null|false
not null|null|false
null|null|true

### deepEquals()
- Objects.deepEquals(Object a, Object b) 역시 두 객체의 동등을 비교하는데, a와 b가 서로 다른 배열일 경우, 항목 값이 모두 같다면 true를 리턴합니다.
- 이것은 Arrays.deepEquals(Object[] a, Object[] b)와 동일합니다.

a|b|Objects.deepEquals(a, b)
:---|:---|:---
not null (not array)|not null (not array)|a.equals(b)의 리턴값
not null (array)|not null (array)|Arrays.deepEquals(a, b)의 리턴값
not null|null|false
null|not null|false
null|null|true
```java
public class EqualsAndDeepExample {
  public static void main(String[] args) {
    Integer o1 = 1000;
    Integer o2 = 1000;
    System.out.println(Objcets.equals(o1, o2));             //true
    System.out.println(Objcets.equals(o1, null));           //false
    System.out.println(Objcets.equals(null, o2));           //false
    System.out.println(Objcets.equals(null, null));         //true
    System.out.println(Objcets.deepEquals(o1, o2) + "\n");  //true
    
    Integer[] arr1 = { 1, 2 };
    Integer[] arr2 = { 1, 2 };
    System.out.println(Objects.equals(arr1, arr2));         //false
    System.out.println(Objects.deepEquals(arr1, arr2));     //true
    System.out.println(Arrays.deepEquals(arr1, arr2));      //true
    System.out.println(Objects.deepEquals(null, arr2));     //false
    System.out.println(Objects.deepEquals(arr1, null));     //false
    System.out.println(Objects.deepEquals(null, null));     //true
  }
}
```

## 해시코드 생성(hash(), hashCode())

### hash()
- Objects.hash(Object... values) 메소드는 매개값으로 주어진 값들을 이용해서 해시 코드를 생성하는 역할을 하는데, 주어진 매개값들로 배열을 생성하고 Arrays.hashCode(Object[])를 호출해서 해시코드를 얻고 이 값을 리턴합니다.
- 이 메소드는 클래스가 hashCode()를 재정의할 때 리턴값을 생성하기 위해 사용하면 좋습니다.
- 클래스가 여러 가지 필드를 가지고 있을 때 이 필드들로부터 해시코드를 생성하게 되면 동일한 필드값을 가지는 객체는 동일한 해시코드를 가질 수 있습니다.
```java
@Override
public int hashCode() {
    return Objects.hash(field1, field2, field3);
}
```

### hashCode()
- Objects.hashCode(Object o)는 매개값으로 주어진 객체의 해시코드를 리턴하기 때문에 o.hashCode()의 리턴값과 동일합니다.
- 차이점은 매개값이 null이면 0을 리턴합니다.
```java
public class HashCodeExample {
  public static void main(String[] args) {
    Student s1 = new Student(1, "홍길동");
    Student s2 = new Student(1, "홍길동");
    System.out.println(s1.hashCode());
    System.out.println(Objects.hashcode(s2));
  }
  
  static class Student {
      int sno;
      String name;
      
      Student(int sno, String name) {
          this.sno = sno;
          this.name = name;
      }
      
      @Override
        public int hashCode() {
          return Objects.hash(sno, name);
      }
  }
}
```

## 널 여부 조사(isNull(), nonNull(), requireNonNull())
- Objects.isNull(Object obj)는 매개값이 null일 경우 true를 리턴합니다.
- 반대로 nonNull(Object obj)는 매개값이 not null일 경우 true를 리턴합니다.
- requireNonNull()는 다음 세 가지로 오버로딩 되어 있습니다.

리턴 타입|메소드(매개 변수)|설명
:---|:---|:---
T|requireNonNull(T obj)|not null -> obj<br>null -> NullPointerException
T|requireNonNull(T obj, String message)|not null -> obj<br>null -> NullPointerException(message)
T|requireNonNull(T obj, Supplier<String> msgSupplier)|not null -> obj<br>null -> NullPointerException(msgSupplier.get())
- 첫 번째 매개값이 not null이면 첫 번째 매개값을 리턴하고, null이면 모두 NullPointerException을 발생시킵니다.
- 두 번째 매개값은 NullPointerException의 예외 메시지를 제공합니다.
```java
public class NullExample {
  public static void main(String[] args) {
    String str1 = "홍길동";
    String str2 = null;

    System.out.println(Objects.requireNonNull(str1));
    
    try {
        String name = Objects.requireNonNull(str2);
    } catch (Exception e) {
      System.out.println(e.getMessage());
    }
    
    try {
        String name = Objects.requireNonNull(str2, "이름이 없습니다.");
    } catch (Exception e) {
      System.out.println(e.getMessage());
    }
    
    try {
        String name = Objects.requireNonNull(str2, () -> "이름이 없다니깐요")
    } catch (Exception e) {
      System.out.println(e.getMessage());
    }
  }
}
```

## 객체 문자 정보(toString())
- Objects.toString()은 객체의 문자 정보를 리턴하는데, 다음 두 가지로 오버로딩되어 있습니다.

리턴 타입|메소드(매개 변수)|설명
:---|:---|:---
String|toString(Object o)|not null -> o.toString()<br>null -> "null"
String|toString(Object o, String nullDefault)|not null -> o.toString()<br>null -> nullDefault
- 첫 번째 매개값이 not null이면 toString()으로 얻은 값을 리턴하고, null이면 "null" 또는 두 번째 매개값인 nullDefault를 리턴합니다.
```java
public class ToStringExample {
  public static void main(String[] args) {
    String str1 = "홍길동";
    String str2 = null;

    System.out.println(Objects.toString(str1));
    System.out.println(Objects.toString(str2));
    System.out.println(Objects.toString(str2, "이름이 없습니다."));
  }
}
```

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

# 10. 정규 표현식과 Pattern 클래스
- 문자열이 정해져 있는 형식으로 구성되어 있는지 검증해야 하는 경우가 있습니다.
- 예를 들어, 이메일, 전화번호를 사용자가 제대로 입력했는지 검증해야 할 때 정규 표현식과 비교합니다.

## 정규 표현식 작성 방법
- 정규 표현식은 문자 또는 숫자 기호와 반복 기호가 결합된 문자열입니다.
<table>
<tr><th>기호</th><th colspan="3">설명</th></tr>
<tr><td rowspan="3">[]</td><td rowspan="3">한 개의 문자</td><td>[abc]</td><td>a, b, c 중 하나의 문자</td></tr>
<tr><td>[^abc]</td><td>a, b, c 이외의 하나의 문자</td></tr>
<tr><td>[a-zA-Z]</td><td>a~z, A~Z 중 하나의 문자</td></tr>
<tr><td>\d</td><td colspan="3">한 개의 숫자, [0-9]와 동일</td></tr>
<tr><td>\s</td><td colspan="3">공백</td></tr>
<tr><td>\w</td><td colspan="3">한 개의 알파벳 또는 한 개의 숫자, [a-zA-Z_0-9]와 동일</td></tr>
<tr><td>?</td><td colspan="3">없음 또는 한 개</td></tr>
<tr><td>*</td><td colspan="3">없음 또는 한 개 이상</td></tr>
<tr><td>+</td><td colspan="3">한 개 이상</td></tr>
<tr><td>{n}</td><td colspan="3">정확히 n개</td></tr>
<tr><td>{n,}</td><td colspan="3">최소한 n개</td></tr>
<tr><td>{n, m}</td><td colspan="3">n개에서부터 m개까지</td></tr>
<tr><td>()</td><td colspan="3">그룹핑</td></tr>
</table>

### 전화번호 정규 표현식
- 02-123-1234 또는 010-1234-5678과 같은 전화번호를 위한 정규 표현식입니다.
```java
(02|010)-\d{3, 4}-\d{4}
```
<table>
<tr><th>기호</th><th>설명</th></tr>
<tr><td>(02|010)</td><td>02 또는 010</td></tr>
<tr><td>-</td><td>- 포함</td></tr>
<tr><td>\d{3, 4}</td><td>3자리 또는 4자리 숫자</td></tr>
<tr><td>-</td><td>- 포함</td></tr>
<tr><td>\d{4}</td><td>4자리 숫자</td></tr>
</table>

### 이메일 정규 표현식
- test@naver.com과 같은 이메일을 위한 정규 표현식입니다.
```java
\w+@\w+\.\w+(\.\w+)?
```
<table>
<tr><th>기호</th><th>설명</th></tr>
<tr><td>\w+</td><td>한 개 이상의 알파벳 또는 숫자</td></tr>
<tr><td>@</td><td>@</td></tr>
<tr><td>\w+</td><td>한 개 이상의 알파벳 또는 숫자</td></tr>
<tr><td>\.</td><td>.</td></tr>
<tr><td>\w+</td><td>한 개 이상의 알파벳 또는 숫자</td></tr>
<tr><td>(\.\w+)?</td><td>\.\w+이 없거나 한 번 더 올 수 있음</td></tr>
</table>

- 주의할 점은 \.과 .은 다른데, \.은 문자로서의 점(.)을 말하지만 .은 모든 문자 중에서 한 개의 문자를 뜻합니다.

## Pattern 클래스
- 문자열을 정규 표현식으로 검증하는 기능은 java.util.regex.Pattern 클래스의 정적 메소드인 matches() 메소드가 제공합니다.
- 첫 번째 매개값은 정규 표현식이고, 두 번째 매개값은 검증할 문자열입니다.
- 검증 후 결과가 boolean 타입으로 리턴됩니다.
```java
public class PatternExample {
  public static void main(String[] args) {
    String regExp = "(02|010)-\\d{3, 4}-\\d{4}";
    String data = "010-123-4567";
    boolean result = Pattern.matches(regExp, data);
    
    if (result) {
      System.out.println("정규식과 일치합니다.");
    } else {
      System.out.println("정규식과 일치하지 않습니다.");
    }
    
    regExp = "\\w+@\\w+\\.\\w+(\\.\\w+)?";
    data = "angel@navercom")    //naver와 com 사이에 .이 없음
    result = Pattern.matches(regExp, data);
    
    if (result) {
      System.out.println("정규식과 일치합니다.");
    } else {
      System.out.println("정규식과 일치하지 않습니다.");
    }
  }
}
```