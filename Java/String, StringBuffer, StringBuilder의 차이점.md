자바에서 문자열을 다루는 클래스로 String, StringBuffer, StringBuilder가 있다.
각 클래스 간의 차이점을 알아보자.

## String과 StringBuffer/StringBuilder의 차이점

String과 StringBuffer/StringBuilder의 가장 큰 차이점은 String은 한번 생성되면 할당된 공간이 변하지 않는 불변(immutable)의 속성을 갖는다. 
반면에, StringBuffer나 StringBuilder의 경우 객체의 공간이 부족해지는 경우 버퍼의 크기를 유연하게 늘려주는 가변(mutable)의 속성을 갖는다.

## StringBuffer와 StringBuilder의 차이점
StringBuffer와 StringBuilder는 모두 가변적인 속성을 가지고 있으며 제공하는 메서드도 동일하다. 
하지만 동기화 지원의 유무에서 차이가 있다. 
StringBuffer는 각 메서드 별로 synchronized 키워드가 존재하여 동기화를 지원한다. 
따라서 멀티 스레드 환경에서 안전하다(thread-safe). 
반면에, StringBuilder는 동기화를 지원하지 않기 때문에 멀티 스레드 환경에서는 적합하지 않다. 
