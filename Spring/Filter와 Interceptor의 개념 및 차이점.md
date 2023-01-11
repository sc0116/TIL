개발을 하다 보면 공통적으로 처리해야 할 업무들이 많다. 
공통 업무에 관련된 코드를 페이지마다 작성하게 되면 중복 코드가 많아지고, 
프로젝트 단위가 커질수록 서버에 부하를 줄 수도 있으며, 소스 관리도 되지 않는다. 
이러한 작업들은 공통 관심사로 보고 분리하는 것이 효율적이다.

Spring은 공통적으로 여러 작업을 처리함으로써 중복된 코드를 제거할 수 있는 기능들을 지원하고 있다.
- Filter(필터)
- Interceptor(인터셉터)
- AOP(Aspect Oriented Programming, 관점 지향 프로그래밍)

이 중에서 필터와 인터셉터의 개념 및 차이점에 대해서 알아보자.

# 필터(Filter)
## 필터란?
필터는 J2EE 표준 스펙 기능으로 디스패처 서블릿(Dispatcher Servlet)에 요청이 전달되기 전/후에 url 패턴에 맞는 모든 요청에 대해 부가 작업을 처리할 수 있는 기능을 제공한다. 
즉, 스프링 컨테이너가 아닌 톰캣과 같은 웹 컨테이너에 의해 관리가 되는 것이고 스프링 범위 밖에서 디스패처 서블릿 전/후에 요청을 처리하는 것이다. 
스프링 빈으로 등록은 가능하다.

![](https://user-images.githubusercontent.com/47477359/211781396-252277ba-9e09-466e-afb1-e4468cccdc7c.png)

## 필터의 메서드
필터를 추가하기 위해서는 javax.servlet의 Filter 인터페이스를 구현해야 하며, 이는 다음 3가지 메서드를 가지고 있다.

```java
public interface Filter {
    
    public default void init(FilterConfig filterConfig) throws ServletException {}
    
    public void doFilter(ServletRequest request, ServletResponse response,
        FilterChain chain) throws IOException, ServletException;
    
    public default void destroy() {}
}
```

### init 메서드
필터 객체를 초기화하고 서비스에 추가하기 위한 메서드이다. 
  웹 컨테이너가 1회 init 메서드를 호출하여 필터 객체를 초기화하면, 이후 요청들은 `doFilter()` 메서드를 통해 처리된다.

### doFilter 메서드
url-pattern에 맞는 모든 HTTP 요청이 디스패처 서블릿으로 전달되기 전에 웹 컨테이너에 의해 실행되고, 
디스패처 서블릿에서 클라이언트에게 HTTP 응답이 가기 전에 웹 컨테이너에 의해 실행되는 메서드이다. 
`doFilter()`의 파라미터로는 FIlterChain이 있는데, FilterChain의 `doFilter()`를 통해 다음 대상으로 요청을 전달하게 된다. 
`chain.doFilter()` 전/후에 우리가 필요한 처리 과정을 넣어줌으로써 원하는 처리를 진행할 수 있다.

### destroy 메서드
필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하기 위한 메서드이다. 
이는 웹 컨테이너에 의해 1번 호출되며, 이후에는 이제 `doFilter()`에 의해 처리되지 않는다.

## 필터의 용도
필터는 기본적으로 스프링과 무관하게 전역적으로 처리해야 하는 작업들을 처리할 수 있다.

- 공통된 보안 관련 작업
  - 필터는 인터셉터보다 앞단인 웹 컨테이너에서 동작하므로 전역적으로 해야 하는 보안 검사(XSS, CSRF 방어 등)를 하여 올바른 요청이 아닐 경우 차단할 수 있다. 
  그러면 스프링 컨테이너까지 요청이 전달되지 못하고 차단되므로 안정성을 더욱 높일 수 있다.
- 모든 요청에 대한 로깅 또는 감사
- 이미지/데이터 압축 및 문자열 인코딩
  - 필터는 웹 애플리케이션에 전반적으로 사용되는 기능을 구현하기에 적당하다.
- Spring과 분리되어야 하는 기능

# 인터셉터(Interceptor)
## 인터셉터란?
인터셉터는 J2EE 표준 스펙인 필터와 달리 Spring이 제공하는 기술로써, 디스패처 서블릿이 컨트롤러를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할 수 있는 기능을 제공한다. 
즉, 웹 컨테이너에서 동작하는 필터와 달리 인터셉터는 스프링 컨텍스트에서 동작한다.

![](https://user-images.githubusercontent.com/47477359/211793974-42978929-ac60-4bf5-958a-15d7871ceaf3.png)

디스패처 서블릿은 핸들러 매핑을 통해 적절한 컨트롤러를 찾도록 요청하는데, 그 결과로 실행 체인(HandlerExecutionChain)을 돌려준다. 
그래서 이 실행 체인은 1개 이상의 인터셉터가 등록되어 있다면 순차적으로 인터셉터들을 거쳐 컨트롤러가 실행되도록 하고, 인터셉터가 없다면 바로 컨트롤러를 실행한다.

## 인터셉터의 메서드
인터셉터를 추가하기 위해서는 org.springframework.web.servlet.HandlerInterceptor 인터페이스를 구현해야 하며, 이는 다음 3가지 메서드를 가지고 있다.

```java
public interface HandlerInterceptor {
    
    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
        throws Exception {
        
        return true;
    }
    
    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
        @Nullable ModelAndView modelAndView) throws Exception {
    }
    
    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
        @Nullable Exception ex) throws Exception {
    }
}
```

### preHandle 메서드
preHandle 메서드는 컨트롤러가 호출되기 전에 실행된다. 
그렇기 때문에 컨트롤러 이전에 처리해야 하는 전처리 작업이나 요청 정보를 가공하거나 추가하는 경우에 사용할 수 있다. 
3번째 파라미터인 handler 파라미터는 @RequestMapping이 붙은 메서드의 정보를 추상화한 객체이다. 
또한 반환 타입은 boolean인데 반환값이 true이면 다음 단계로 진행 되지만, false라면 작업을 중단하여 이후의 작업(다음 인터셉터 또는 컨트롤러)은 진행되지 않는다.

### postHandle 메서드
postHandle 메서드는 컨트롤러를 호출된 후에 실행된다. 
그렇기 때문에 컨트롤러 이후에 처리해야 하는 후처리 작업이 있을 때 사용할 수 있다.

### afterCompletion 메서드
afterCompletion 메서드는 이름 그대로 모든 뷰에서 최종 결과를 생성하는 일을 포함해 모든 작업이 완료된 후에 실행된다. 

## 인터셉터의 용도
인터셉터는 클라이언트의 요청과 관련되어 전역적으로 처리해야 하는 작업들을 처리할 수 있다.

- 세부적인 인증/인가 공통 작업
  - 대표적으로 세부적으로 적용해야 하는 인증이나 인가와 같이 클라이언트 요청과 관련된 작업 등이 있다. 
  예를 들어 특정 그룹의 사용자는 어떤 기능을 사용하지 못하는 경우가 있는데, 이러한 작업들은 컨트롤러로 넘어가기 전에 검사해야 하므로 인터셉터가 처리하기에 적합하다.
- API 호출에 대한 로깅 또는 감사
  - 전달 받는 HttpServletRequest, HttpServletResponse 객체를 통해 클라이언트의 IP나 요청 정보들을 기록할 수 있다.
- Controller로 넘겨주는 정보(데이터)의 가공
  - 필터와 다르게 HttpServletRequest나 HttpServletResponse 등과 같은 객체를 제공받으므로 객체 자체를 조작할 수는 없지만, 
  해당 객체가 내부적으로 갖는 값은 조작할 수 있으므로 컨트롤러로 넘겨주기 위한 정보를 가공할 수 있다.

# 필터와 인터셉터의 차이점
- 필터는 웹 컨테이너에서 관리되는 반면, 인터셉터는 스프링 컨테이너에서 관리된다.
- 필터는 디스패처 서블릿의 전/후를 다룰 수 있는 반면, 인터셉터는 컨트롤러의 전/후를 다룰 수 있다.
- Request/Response 객체 조작 가능 여부
  - 필터는 다음 필터를 호출하기 위해서는 필터 체이닝(다음 필터 호출)을 해주어야 한다. 
  그리고 이때 Request/Response 객체를 넘겨주므로 우리가 원하는 Request/Response 객체를 넣어줄 수 있다.
  - 인터셉터는 디스패처 서블릿이 여러 인터셉터 목록을 가지고 있고, for문으로 순차적으로 실행시킨다. 
  그리고 true를 반환하면 다음 인터셉터가 실행되거나 컨트롤러로 요청이 전달되며, false가 반환되면 요청이 중단된다. 
  그러므로 다른 Request/Response 객체를 넘겨줄 수 없다.

## 참고
[https://mangkyu.tistory.com/173](https://mangkyu.tistory.com/173)
