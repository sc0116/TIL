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

# 5. System 클래스
- 자바 프로그램은 운영체제상에서 바로 실행되는 것이 아니라 JVM 위에서 실행되기 때문에 운영체제의 모든 기능을 자바 코드로 직접 접근하기란 어렵습니다.
- 하지만 java.lang 패키지에 속하는 System 클래스를 이용하면 운영체제의 일부 기능을 이용할 수 있습니다. (프로그램 종료, 키보드 입력, 모니터 출력, 메모리 정리, 현재 시간 읽기, 시스템 프로퍼티 읽기, 환경 변수 읽기 등)
- System 클래스의 모든 필드와 메소드는 정적 필드와 정적 메소드로 구성되어 있습니다.

## 프로그램 종료(exit())
- exit() 메소드는 현재 실행하고 있는 프로세스를 **강제 종료**시키는 역할을 합니다.
- exit() 메소드는 int 매개값을 지정하도록 되어 있는데, 이 값을 종료 상태값이라고 합니다.
- 일반적으로 정상 종료일 경우 0으로 지정하고 비정상 종료일 경우 0 이외의 다른 값을 줍니다.
```java
System.exit(0);
```
- 만약 특정 값이 입력되었을 경우에만 종료하고 싶다면 자바의 보안 관리자를 직접 설정해서 종료 상태값을 확인하면 됩니다.
- System.exit()가 실행되면 보안 관리자의 checkExit() 메소드가 자동 호출되는데, 이 메소드에서 종료 상태값을 조사해서 특정 값이 입력되지 않으면 SecurityException을 발생시켜 System.exit()를 호출한 곳에서 예외 처리를 할 수  있도록 해줍니다.
```java
public class ExitExample {
  public static void main(String[] args) {
    //보안 관리자 설정
    System.setSecurityManaager(new SecurityManager() {
      @Override
      public void checkExit(int status) {
        if (status != 5) {
          throw new SecurityException();
        }
      }
    });
    
    for (int i = 0; i < 10; i++) {
      System.out.println(i);
      try {
          System.exit(i);
      } catch (SecurityException e) { }
    }
  }
}
```

## 쓰레기 수집기 실행(gc())
- 자바는 개발자가 메모리를 직접 코드로 관리하지 않고 JVM이 알아서 자동으로 관리합니다.
- JVM은 메모리가 부족할 때와 CPU가 한가할 때에 쓰레기 수집기(Garbage Collector)를 실행시켜 사용하지 않는 객체를 자동 제거합니다.
- 쓰레기 수집기는 개발자가 직접 코드로 실행시킬 수 없고, JVM에게 **가능한한 빨리 실행해 달라고 요청**할 수는 있습니다.
- System.gc() 메소드가 호출되면 쓰레기 수집기가 바로 실행되는 것은 아니고, JVM은 빠른 시간 내에 실행시키기 위해 노력합니다.
```java
System.gc();
```

## 현재 시각 읽기(currentTimeMillis(), nanoTime())
- currentTimeMillis() 메소드와 nanoTime() 메소드는 컴퓨터의 시계로부터 현재 시간을 읽어서 밀리세컨드(1/1000초) 단위와 나노세컨드(1/10^9초) 단위의 long 값을 리턴합니다.
```java
long time = System.currentTimeMillis();
long time = System.nanoTime();
```
- 리턴값은 주로 프로그램의 실행 소요 시간 측정에 사용됩니다.
```java
public class SystemTimeExample {
  public static void main(String[] args) {
    long time1 = System.nanoTime();
    
    int sum = 0;
    for (int i = 1; i <= 1000000; i++) {
        sum += i;
    }
    
    long time2 = System.nanoTime();

    System.out.println("1 ~ 1000000까지의 합: " + sum);
    System.out.println("계산에 " + (time2 - time1) + " 나노초가 소요되었습니다.");
  }
}
```

## 시스템 프로퍼티 읽기(getProperty())
- 시스템 프로퍼티는 JVM이 시작할 때 자동 설정되는 시스템의 속성값을 말하며, 키와 값으로 구성되어 있습니다.

키(key)|설명|값(value)
:---|:---|:---
java.version|자바의 버전|1.8.0_20
java.home|사용하는 JRE의 파일 경로|<jdk 설치경로>\jre
os.name|Operating System name|Windows 10
file.separator|File separator ("/" on UNIX)|\
user.name|사용자의 이름|사용자계정
user.home|사용자의 홈 디렉토리|C:\Users\사용자계정
user.dir|사용자가 현재 작업 중인 디렉토리 경로|다양
- 시스템 프로퍼티를 읽어오기 위해서는 System.getProperty() 메소드를 이용하면 됩니다.
- 이 메소드는 시스템 프로퍼티의 키 이름을 매개값으로 받고, 해당 키에 대한 값을 문자열로 리턴합니다.
```java
String value = System.getProperty(String key);
```

## 환경 변수 읽기(getenv())
- 대부분의 운영체제는 실행되는 프로그램들에게 유용한 정보를 제공할 목적으로 환경 변수를 제공합니다.
- 환경 변수는 프로그램상의 변수가 아니라 운영체제에서 이름과 값으로 관리되는 문자열 정보입니다.
- 자바 프로그램에서는 환경 변수의 값이 필요할 경우 System.getenv() 메소드를 사용합니다.
- 매개값으로 환경 변수 이름을 주면 값을 리턴합니다.
```java
public class SystemEnvExample {
  public static void main(String[] args) {
    String javaHome = System.getenv("JAVA_HOME");
    System.out.println("JAVA_HOME: " + javaHome);
  }
}
```

# 6. Class 클래스
- 자바는 클래스와 인터페이스의 메타 데이터를 java.lang 패키지에 소속된 Class 클래스로 관리합니다.
- 메타 데이터란 클래스의 이름, 생성자 정보, 필드 정보, 메소드 정보를 말합니다.

## Class 객체 얻기(getClass(), forName())
- 프로그램에서 Class 객체를 얻기 위해서는 모든 클래스의 최상위 클래스인 Object 클래스의 getClass() 메소드를 호출하면 됩니다.
```java
Class clazz = obj.getClass();
```
- getClass() 메소드는 해당 클래스로 객체를 생성했을 때만 사용할 수 있는데, 객체를 생성하기 전에 직접 Class 객체를 얻을 수도 있습니다.
- Class는 생성자를 감추고 있기 때문에 new 연산자로 객체를 만들 수 없고, 정적 메소드인 forName()을 이용해야 합니다.
- forName() 메소드는 클래스 전체 이름(패키지가 포함된 이름)을 매개값으로 받고 Class 객체를 리턴합니다.
```java
try {
    Class clazz = Class.forName(String className);
} catch (ClssNotFoundException e) {
}
```
- Class.forName() 메소드는 매개값으로 주어진 클래스를 찾지 못하면 ClassNotFoundException 예외를 발생시키기 때문에 예외 처리가 필요합니다.
```java
public class ClassExample {
  public static void main(String[] args) {
    Car car = new Car();
    Class clazz1 = car.getClass();
    System.out.println(clazz1.getName());                       //sec06.exam01_class.Car
    System.out.println(clazz1.getSimpleName());                 //Car
    System.out.println(clazz1.getPackage().getName() + "\n");   //sec06.exam01_class
    
    try {
        Class clazz2 = Class.forName("sec06.exam01_class.Car");
      System.out.println(clazz2.getName());                     //sec06.exam01_class.Car
      System.out.println(clazz2.getSimpleName());               //Car
      System.out.println(clazz2.getPackage().getName());        //sec06.exam01_class
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
  }
}
```

## 리플렉션(getDeclaredConstructors(), getDeclaredFields(), getDeclaredMethods())
- Class 객체를 이용하면 클래스의 생성자, 필드, 메소드 정보를 알아낼 수 있고, 이것을 리플렉션(Reflection)이라고 합니다.
```java
Constructor[] constructors = clazz.getDeclaredConstructors();
Field[] fiels = clazz.getDeclaredFields();
Method[] methods = clazz.getDeclaredMethods();
```
- 세 메소드는 각각 Constructor 배열, Field 배열, Method 배열을 리턴합니다.
- Constructor, Field, Method 클래스는 모두 java.lang.reflect 패키지에 소속되어 있습니다.
- getDeclaredFields(), getDeclaredMethods()는 클래스에 선언된 멤버만 가져오고 상속된 멤버는 가져오지 않습니다.
- 만약 상속된 멤버도 얻고 싶다면 getFields(), getMethods()를 이용해야 합니다. 단 public 멤버만 가져옵니다.
```java
public class ReflectionExample {
  public static void main(String[] args) {
    Class clazz = Class.forName("sec06.exam02_reflection.Car");

    System.out.println("[클래스 이름]");
    System.out.println(clazz.getName() + "\n");

    System.out.println("[생성자 정보]");
    Constructor[] constructors = clazz.getDeclaredConstructors();
    for (Constructor constructor : constructors) {
      System.out.print(constructor.getName() + "(");
      Class[] parameters = constructor.getParameterTypes();
      printParameters(parameters);
      System.out.println(")");
    }
    System.out.println();

    System.out.println("[필드 정보]");
    Field[] fields = clazz.getDeclaredFields();
    for (Field field : fields) {
      System.out.println(field.getType().getSimpleName() + " " + field.getName());
    }
    System.out.println();

    System.out.println("[메소드 정보]");
    Method[] methods = clazz.getDeclaredMethods();
    for (Method method : methods) {
      System.out.print(method.getName() + "(");
      Class[] parameters = method.getParameterTypes();
      printParameters(parameters);
      System.out.println(")");
    }
  }
  
  private static void printParameters(Class[] parameters) {
      for (int i = 0; i < parameters.length; i++) {
        System.out.println(parameters[i].getName());
        if (i < parameters.length - 1) {
          System.out.print(",");
        }
      }
  }
}
```

## 동적 객체 생성(newInstance())
- Class 객체를 이용하면 new 연산자를 사용하지 않아도 동적으로 객체를 생성할 수 있습니다.
- 이 방법은 코드 작성 시에 클래스 이름을 결정할 수 없고, 런타임 시에 클래스 이름이 결정되는 경우에 매우 유용하게 사용됩니다.
```java
try {
    Class clazz = Class.forName("런타임 시 결정되는 클래스 이름");
    Objcet obj = clazz.newInstance();
} catch (ClassNotFoundException e) {
} catch (InstantiationException e) {    //해당 클래스가 추상 클래스이거나 인터페이스일 경우 발생
} catch (IllegalAccessException e) {    //클래스나 생성자가 접근 제한자로 인해 접근할 수 없을 경우 발생
} 
```
- newInstance() 메소드는 기본 생성자를 호출해서 객체를 생성하기 때문에 **반드시 클래스에 기본 생성자가 존재**해야 합니다.
- 만약 매개 변수가 있는 생성자를 호출하고 싶다면 리플렉션으로 Constructor 객체를 얻어 newInstance() 메소드를 호출하면 됩니다.
- newInstance() 메소드의 리턴 타입은 Object이므로 이것을 원래 클래스 타입으로 변환해야만 메소드를 사용할 수 있습니다.
- 그렇게 하기 위해서는 강제 타입 변환을 해야 하는데, 클래스 타입을 모르는 상태이므로 변환을 할 수가 없습니다.
- 이 문제를 해결하려면 인터페이스 사용이 필요합니다.
```java
public interface Action {
    public void execute();
}

public class SendAction implements Action {
    @Override
    public void execute() {
      System.out.println("데이터를 보냅니다.");
    }
}

public class ReceiveAction implements Action {
    @Override
    public void execute() {
      System.out.println("데이터를 받습니다.");
    }
}

public class NewInstanceExample {
  public static void main(String[] args) {
    try {
        Class clazz = Class.forName("sec06.exam03_newinstance.SendAction");
        //Class clazz = Class.forName("sec06.exam03_newinstance.ReceiveAction");
        Action action = (Action) clazz.newInstance();
        action.execute();
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (InstantiationException e) {
        e.printStackTrace();
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    }
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

# 11. Arrays 클래스
- Arrays 클래스는 배열 조작 기능을 가지고 있습니다.
- 단순한 배열 복사는 System.arraycopy() 메소드를 사용할 수 있으나, Arrays는 추가적으로 항목 정렬, 항목 검색, 항목 비교와 같은 기능을 제공해줍니다.

리턴 타입|메소드 이름|설명
:---|:---|:---
int|binarySearch(배열, 찾는 값)|전체 배열 항목에서 찾는 값이 있는 인덱스 리턴
타겟 배열|copyOf(원본 배열, 복사할 길이)|원본 배열의 0번 인덱스에서 복사할 길이만큼 복사한 배열 리턴, 복사할 길이는 원본 배열의 길이보다 커도 되며, 타겟 배열의 길이가 된다.
타겟 배열|copyOfRange(원본 배열, 시작 인덱스, 끝 인덱스)|원본 배열의 시작 인덱스에서 끝 인덱스까지 복사한 배열 리턴
boolean|deepEquals(배열, 배열)|두 배열의 깊은 비교(중첩 배열의 항목까지 비교)
boolean|equals(배열, 배열)|두 배열의 얕은 비교(중첩 배열의 항목은 비교하지 않음)
void|fill(배열, 값)|전체 배열 항목에 동일한 값을 저장
void|fill(배열, 시작 인덱스, 끝 인덱스, 값)|시작 인덱스부터 끝 인덱스까지의 항목에만 동일한 값을 저장
void|sort(배열)|배열의 전체 항목을 오름차순으로 정렬
String|toString(배열)|"[값1, 값2, ...]"와 같은 문자열 리턴

## 배열 복사
- 배열 복사를 위해 사용할 수 있는 메소드는 copyOf(원본 배열, 복사할 길이), copyOfRange(원본 배열, 시작 인덱스, 끝 인덱스)입니다.
- copyOf() 메소드는 원본 배열의 0번 인덱스에서 복사할 길이만큼 복사한 타겟 배열을 리턴하는데, 복사할 길이는 원본 배열의 길이보다 커도 되며, 타겟 배열의 길이가 됩니다.
- copyOfRange() 메소드는 원본 배열의 시작 인덱스에서 끝 인덱스까지 복사한 배열을 리턴하는데, 시작 인덱스는 포함되지만 끝 인덱스는 포함되지 않습니다.
- 단순히 배열을 복사할 목적이라면 System.arraycopy() 메소드를 이용할 수 있습니다.
```java
System.arraycopy(원본 배열, 원본 시작 인덱스, 타겟 배열, 타겟 시작 인덱스, 복사 개수)
```
```java
public class ArrayCopyExample {
  public static void main(String[] args) {
    char[] arr1 = {'j', 'a', 'v', 'a'};
    
    //방법 1
    char[] arr2 = Arrays.copyOf(arr1, arr1.length);
    System.out.println(Arrays.toString(arr2));  //[j, a, v, a]
    
    //방법 2
    char[] arr3 = Arrays.copyOfRange(arr1, 1, 3);
    System.out.println(Arrays.toString(arr3));  //[a, v]
    
    //방법 3
    char[] arr4 = new char[arr1.length];
    System.arraycopy(arr1, 0, arr4, 0, arr1.length);
    for (int i = 0; i < arr4.length; i++) {
      System.out.println("arr4[" + i + "]=" + arr4[i]);
    }
  }
}
```

## 배열 항목 비교
- Arrays의 equals()와 deepEquals()는 배열 항목을 비교합니다.
- equals()는 1차 항목의 값만 비교하고, deepEquals()는 1차 항목이 서로 다른 배열을 참조할 경우 중첩된 배열의 항목까지 비교합니다.
```java
public class EqualsExample {
  public static void main(String[] args) {
    int[][] original = { {1, 2}, {3, 4} };
    
    //얕은 복사 후 비교
    System.out.println("[얕은 복제 후 비교]");
    int[][] cloned1 = Arrays.copyOf(original, original.length);
    System.out.println("배열 번지 비교: " + original.equals(cloned1));                       //false
    System.out.println("1차 배열 항목 값 비교: " + Arrays.equals(original, cloned1));        //true
    System.out.println("중첩 배열 항목 값 비교: " + Arrays.deepEquals(original, cloned1));   //true
    
    //깊은 복사 후 비교
    System.out.println("[깊은 복제 후 비교]");
    int[][] cloned2 = Arrays.copyOf(original, original.length);
    cloned2[0] = Arrays.copyOf(original[0], original[0].length);
    cloned2[1] = Arrays.copyOf(original[1], original[1].length);
    System.out.println("배열 번지 비교: " + original.equals(cloned2));                       //false
    System.out.println("1차 배열 항목 값 비교: " + Arrays.equals(original, cloned2));        //false
    System.out.println("중첩 배열 항목 값 비교: " + Arrays.deepEquals(original, cloned2));   //true
  }
}
```

## 배열 항목 정렬
- 기본 타입 또는 String 배열은 Arrays.sort() 메소드의 매개값으로 지정해주면 자동으로 오름차순 정렬이 됩니다.
- 사용자 정의 클래스 타입일 경우에는 클래스가 Comparable 인터페이스를 구현하고 있어야 정렬이 됩니다.
```java
public class Member implements Comparable<Member> {
    String name;
    Member(String name) {
        this.name = name;
    }
    
    @Override
    public int compareTo(Member o) {
        return name.compareTo(o.name);
    }
}

public class SortExample {
  public static void main(String[] args) {
    int[] scores = { 99, 97, 98 };
    Arrays.sort(scores);
    for (int i = 0; i < scores.length; i++) {
      System.out.println("scores[" + i + "]=" + scores[i]); // 97 98 99
    }
    System.out.println();
    
    String[] names = { "홍길동", "박동수", "김민수" };
    Arrays.sort(names);
    for (int i = 0; i < names.length; i++) {
      System.out.println("names[" + i + "]=" + names[i]);   //김민수 박동수 홍길동
    }
    System.out.println();
    
    Member m1 = new Member("홍길동");
    Member m1 = new Member("박동수");
    Member m1 = new Member("김민수");
    Member[] members = { m1, m2, m3 };
    Arrays.sort(members);
    for (int i = 0; i < members.length; i++) {
      System.out.println("members[" + i + "].name=" + members[i].name); //김민수 박동수 홍길동
    }
  }
}
```

## 배열 항목 검색
- 배열 항목에서 특정 값이 위치한 인덱스를 얻는 것을  배열 검색이라고 합니다.
- 배열 항목을 검색하려면 먼저 Arrays.sort() 메소드로 항목들을 오름차순으로 정렬한 후, Arrays.binarySearch() 메소드로 항목을 찾아야 합니다.
```java
public class SearchExample {
  public static void main(String[] args) {
    int[] scores = { 99, 97, 98 };
    Arrays.sort(scores);
    int index = Arrays.binarySearch(scores, 99);
    System.out.println("찾은 인데스: " + index); //2
  }
}
```

# 12. Wrapper(포장) 클래스
- 자바는 기본 타입(byte, char, short, int, long, float, double, boolean)의 값을 갖는 객체를 생성할 수 있고, 이런 객체를 포장(Wrapper) 객체라고 합니다.
- 포장 객체의 특징은 포장하고 있는 기본 타입 값은 외부에서 변경할 수 없으며, 내부의 값을 변경하고 싶다면 새로운 포장 객체를 만들어야 합니다.
- 포장 클래스는 java.lang 패키지에 포함되어 있습니다.

기본 타입|포장 클래스
byte|Byte
char|Character
short|Short
int|Integer
long|Long
float|Float
double|Double
boolean|Boolean

## 박싱(Boxing)과 언박싱(Unboxing)
- 기본 타입의 값을 포장 객체로 만드는 과정을 박싱(Boxing)이라고 하고, 반대로 포장 객체에서 기본 타입의 값을 얻어내는 과정을 언박싱(Unboxing)이라고 합니다.
- 박싱하는 방법은 간단하게 포장 클래스의 생성자 매개값으로 기본 타입의 값 또는 문자열을 넘겨주면 됩니다.

기본 타입의 값을 줄 경우|문자열을 줄 경우
:---|:---
Byte obj = new Byte(10);|Byte obj = new Byte("10");
Character obj = new Character('가');|없음
Short obj = new Short(100);|Short obj = new Short("100");
Integer obj = new Integer(1000);|Integer obj = new Integer("1000");
Long obj = new Long(10000);|Long obj = new Long("10000");
Float obj = new Float(2.5F)|Float obj = new Float("2.5F");
Double obj = new Double(3.5);|Double obj = new Double("3.5");
Boolean obj = new Boolean(true);|Boolean obj = new Boolean("true");
- 생성자를 이용하지 않아도 다음과 같이 각 포장 클래스마다 가지고 있는 정적 valueOf() 메소드를 사용할 수도 있습니다.
```java
Integer obj = Integer.valueOf(1000);
Integer obj = Integer.valueOf("1000");
```
- 박싱된 포장 객체에서 다시 기본 타입의 값을 얻어 내기 위해서는(언박싱하기 위해서는) 각 포장 클래스마다 가지고 있는 "기본 타입명 + Value()" 메소드를 호출하면 됩니다.

기본 타입의 값을 이용|
:---|
byte num = obj.byteValue();|
char ch = obj.charValue();|
short num = obj.shortValue();|
int num = obj.intValue();|
long num = obj.longValue();|
float num = obj.floatValue();|
double num = obj.doubleValue();|
boolean bool = obj.booleanValue();|
```java
public class BoxingUnBoxingExample {
  public static void main(String[] args) {
    //Boxing
    Integer obj1 = new Integer(100);
    Integer obj2 = new Integer("200");
    Integer obj3 = Integer.valueOf("300");
    
    //Unboxing
    int value1 = obj1.intValue();
    int value2 = obj2.intValue();
    int value3 = obj3.intValue();

    System.out.println(value1);
    System.out.println(value2);
    System.out.println(value3);
  }
}
```

## 자동 박싱과 언박싱
- 자동 박싱은 포장 클래스 타입에 기본값이 대입될 경우에 발생합니다.
```java
Integer obj = 100;  //자동 박싱
```
- 자동 언박싱은 기본 타입에 포장 객체가 대입될 경우에 발생합니다.
```java
Integer obj = new Integer(200);
int value1 = obj;       //자동 언박싱
int value2 = obj + 100; //자동 언박싱
```

## 문자열을 기본 타입 값으로 변환
- 포장 클래스의 주요 용도는 기본 타입의 값을 박싱해서 포장 객체로 만드는 것이지만, 문자열을 기본 타입 값으로 변환할 때에도 많이 사용됩니다.
- 대부분의 포장 클래스에는 "parse + 기본 타입명"으로 되어 있는 정적(static) 메소드가 있습니다.
- 이 메소드는 문자열을 매개값으로 받아 기본 타입 값으로 변환합니다.

기본 타입의 값을 이용|
:---|
byte num = Byte.parseByte("10");|
short num = Short.parseShort("100");|
int num = Integer.parseInt("1000");|
long num = Long.parseLong("10000");|
float num = Float.parseFloat("2.5F");|
double num = Double.parseDouble("3.5")|
boolean bool = Boolean.parseBoolean("true");|
```java
public class StringToPrimitiveValueExample {
  public static void main(String[] args) {
    int value1 = Integer.parseInt("10");
    double value2 = Double.parseDouble("3.14");
    boolean value3 = Boolean.parseBoolean("true");

    System.out.println("value1: " + value1);
    System.out.println("value2: " + value2);
    System.out.println("value3: " + value3);
  }
}
```

## 포장 값 비교
- 포장 객체는 내부의 값을 비교하기 위해 ==와 != 연산자를 사용할 수 없습니다.
- 이 연산자는 내부의 값을 비교하는 것이 아니라 포장 객체의 참조를 비교하기 때문입니다.
- 내부의 값만 비교하려면 언박싱한 값을 얻어 비교해야 하지만, 박싱된 값이 다음 표에 나와 있는 값이라면 ==와 != 연산자로 내부의 값을 바로 비교할 수 있습니다.

타입|값의 범위
:---|:---
boolean|true, false
char|\u0000 ~ \u007f
byte, short, int|-128 ~ 127
- 포장 객체에 정확히 어떤 값이 저장될 지 모르는 상황이라면 ==와 != 연산자는 사용하지 않는 것이 좋습니다.
- 직접 내부 값을 언박싱해서 비교하거나, equals() 메소드로 내부 값을 비교하는 것이 좋습니다.
- 포장 클래스의 equals() 메소드는 내부의 값을 비교하도록 오버라이딩되어 있습니다.
```java
public class ValueCompareExample {
  public static void main(String[] args) {
    System.out.println("[-128 ~ 127 초과값일 경우]");
    Integer obj1 = 300;
    Integer obj2 = 300;
    System.out.println("== 결과: " + (obj1 == obj2));                                //false
    System.out.println("언박싱 후 == 결과: " + (obj1.intValue() == obj2.intValue())); //true
    System.out.println("equals() 결과: " + obj1.equals(obj2));                      //true

    System.out.println("[-128 ~ 127 범위값일 경우]");
    Integer obj3 = 10;
    Integer obj4 = 10;
    System.out.println("== 결과: " + (obj3 == obj4));                                //true
    System.out.println("언박싱 후 == 결과: " + (obj3.intValue() == obj4.intValue())); //true
    System.out.println("equals() 결과: " + obj3.equals(obj4));                      //trueZk
  }
}
```

# 13. Math, Random 클래스

## Math 클래스
- java.lang.Math 클래스는 수학 계산에 사용할 수 있는 메소드를 제공하고 있습니다.
- Math 클래스가 제공하는 메소드는 모두 정적(static)이므로 Math 클래스로 바로 사용이 가능합니다.

메소드|설명|예제 코드|리턴값
:---|:---|:---|:---
int abs(int a)<br>double abs(double a)|절대값|int v1 = Math.abs(-5);<br>double v2 = Math.abs(-3.14);|v1 = 5<br>v2 = 3.14
double ceil(double a)|올림값|double v3 = Math.ceil(5.3);<br>double v4 = Math.ceil(-5.3);|v3 = 6.0<br>v4 = -5.0
double floor(double a)|버림값|double v5 = Math.floor(5.3);<br>double v6 = Math.floor(-5.3);|v5 = 5.0<br>v6 = -6.0
int max(int a, int b)<br>double max(double a, double b)|최대값|int v7 = Math.max(5, 9);<br>double v8 = Math.max(5.3, 2.5);|v7 = 9<br>v8 = 5.3
int min(int a, int b)<br>double min(double a, double b)|최소값|int v9 = Math.min(5, 9);<br>double v10 = Math.min(5.3, 2.5);|v9 = 5<br>v10 = 2.5
double random()|랜덤값|double v11 = Math.random()|0.0 <= v11 <= 1.0
double rint(double a)|가까운 정수의 실수값|double v12 = Math.rint(5.3);<br>double v13 = Math.rint(5.7);|v12 = 5.0<br>v13 = 6.0
long round(double a)|반올림값|long v14 = Math.round(5.3);<br>long v15 = Math.round(5.7);|v14 = 5<br>v15 = 6
- round() 메소드는 항상 소수점 첫째 자리에서 반올림해서 정수값을 리턴합니다.
- 만약 원하는 소수 자릿수에서 반올림된 값을 얻기 위해서는 반올림할 자릿수가 소수점 첫째 자리가 되도록 10^n을 곱한 후, round() 메소드의 리턴값을 얻고, 다시 10^n.0을 나눠주면 됩니다.
- Math.random() 메소드는 0.0과 1.0 사이의 범위에 속하는 하나의 double 타입의 값을 리턴합니다.
```java
0.0 <= Math.random() < 1.0

//주사위 번호 뽑기
int num = (int) (Math.random() * 6) + 1;

//로또 번호 뽑기
int num = (int) (Math.random() * 45) + 1;
```

## Random 클래스
- java.util.Random 클래스는 난수를 얻어내기 위해 다양한 메소드를 제공합니다.
- Math.random() 메소드는 0.0에서 1 사이의 double 난수를 얻는 데만 사용한다면, Random 클래스는 boolean, int, long, float, double 난수를 얻을 수 있습니다.
- 또 다른 차이점은 Random 클래스는 종자값(seed)을 설정할 수 있습니다.
- 종자값은 난수를 만드는 알고리즘에 사용되는 값으로 종자값이 같으면 같은 난수를 얻습니다.

생성자|설명
:---|:---
Random()|호출 시마다 다른 종자값(현재시간 이용)이 자동 설정된다.
Random(long seed)|매개값으로 주어진 종자값이 설정된다.

### Random 클래스가 제공하는 메소드

리턴값|메소드(매개 변수)|설명
:---|:---|:---
boolean|nextBoolean()|boolean 타입의 난수를 리턴
double|nextDouble()|double 타입의 난수를 리턴(0.0 <= ~ < 1.0)
int|nextInt()|int 타입의 난수를 리턴(-2^31 <= ~ <= 2^31 - 1)
int|nextInt(int n)|int 타입의 난수를 리턴(0 <= ~ < n)