트랜잭션이란 데이터베이스의 상태를 변환시키는 하나의 논리적 기능을 수행하기 위한 작업의 단위 또는 한꺼번에 모두 수행되어야 할 일련의 연산들을 의미한다. 트랜잭션은 이러한 논리적인 작업 단위가 모두 적용되거나 오류가 발생한 경우 모두 적용되지 않아야 함을 보장해주는 것이다.  
스프링은 '@Transactional' 어노테이션을 이용한 선언적 트랜잭션 처리를 지원한다. '@Transactional'은 해당 메서드를 하나의 트랜잭션 안에서 진행할 수 있도록 만들어주는 역할을 한다.
이때 트랜잭션 내부에서 트랜잭션을 또 호출한다면 스프링에서는 어떻게 처리할까?

# @Transactional Propagation

트랜잭션 전파란 트랜잭션의 경계에서 이미 진행 중인 트랜잭션이 있을 때 또는 없을 때 어떻게 동작할 것인가를 결정하는 방식을 말한다.
트랜잭션의 전파 속성은 7가지가 있다.

- **REQUIRED**: Support a current transaction, create a new one if none exists.
- **REQUIRES_NEW**: Create a new transaction, and suspend the current transaction if one exists.
- **NESTED**: Execute within a nested transaction if a current transaction exists, behave like REQUIRED otherwise.
- **MANDATORY**: Support a current transaction, throw an exception if none exists.
- **SUPPORTS**: Support a current transaction, execute non-transactionally if none exists.
- **NOT_SUPPORTED**: Execute non-transactionally, suspend the current transaction if one exists.
- **NEVER**: Execute non-transactionally, throw an exception if a transaction exists.

## Propagation.REQUIRED

기본값이기 때문에 생략이 가능하다.  
**REQUIRED** 전파 속성은 이미 진행 중인 트랜잭션(부모 트랜잭션)이 존재한다면 부모 트랜잭션으로 합류하지만, 부모 트랜잭션이 존재하지 않는다면 자식 트랜잭션은 새로운 트랜잭션을 생성하는 방식이다.  
부모 또는 자식 트랜잭션에서 중간에 롤백이 발생한다면 부모와 자식 트랜잭션 모두 하나의 트랜잭션이기 때문에 진행사항이 모두 롤백된다.

![](https://velog.velcdn.com/images/sc_shin/post/2b45131e-5440-43ac-a5a3-5efaad269acb/image.png)

## Propagation.REQUIRES_NEW

**REQUIRES_NEW** 전파 속성은 매번 새로운 트랜잭션을 시작하는 방식이다.  
이미 진행중인 트랜잭션이 존재한다면 기존의 트랜잭션은 메서드가 종료할 때까지 잠시 대기 상태로 두고 새로 시작한 트랜잭션을 실행한다.

![](https://velog.velcdn.com/images/sc_shin/post/18b1c941-b4da-41d0-bc84-6e37bbb577e7/image.png)

## Propagation.NESTED

**NESTED** 전파 속성은 부모 트랜잭션이 있다면 중첩 트랜잭션을 생성하는 방식이다.  
중첩된 트랜잭션 내부에서 롤백이 발생하는 경우 중첩 트랜잭션의 시작 지점 까지만 롤백된다.  
중첩 트랜잭션이 끝난 후 부모 트랜잭션에서 롤백이 발생하는 경우 모든 트랜잭션이 롤백된다.  
중첩 트랜잭션은 부모 트랜잭션이 커밋될 때 같이 커밋된다.  
만약 부모 트랜잭션이 존재하지 않는다면 새로운 트랜잭션을 생성한다.

## Propagation.MANDATORY

**MANDATORY** 전파 속성은 무조건 부모 트랜잭션에 합류하는 방식이다.  
부모 트랜잭션이 있다면 합류하지만, 부모 트랜잭션이 없다면 예외를 발생시킨다.

## Propagation.SUPPORTS

**SUPPORTS** 전파 속성은 부모 트랜잭션이 있다면 메서드가 트랜잭션을 필요로 하지 않더라도 합류하지만, 부모 트랜잭션이 없다면 트랜잭션 생성하지 않고 진행하는 방식이다.  

## Propagation.NOT_SUPPORTED

**NOT_SUPPORTED** 전파 속성은 부모 트랜잭션이 있다면 일시 정지시키고 트랜잭션 없이 실행되도록 하지만, 부모 트랜잭션이 없다면 트랜잭션을 생성하지 않고 진행하는 방식이다.

## Propagation.NEVER

**NEVER** 전파 속성은 부모 트랜잭션이 있다면 예외를 발생시키지만, 부모 트랜잭션이 없다면 트랜잭션을 생성하지 않고 진행하는 방식이다.
