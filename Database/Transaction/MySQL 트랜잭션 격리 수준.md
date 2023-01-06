# 트랜잭션 격리 수준
트랜잭션의 격리 수준(isolation level)이란 여러 트랜잭션이 동시에 처리될 때 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있게 허용할지 말지를 결정하는 것이다.
격리 수준은 크게 다음과 같이 4가지로 구분된다.

- READ UNCOMMITTED(커밋되지 않은 읽기)
- READ COMMITTED(커밋된 읽기)
- REPEATABLE READ(반복 가능한 읽기)
- SERIALIZABLE(직렬화 가능)

4개의 격리 수준에서 순서대로 뒤로 갈수록 각 트랜잭션 간의 데이터 격리 정도가 높아지며, 동시 처리 성능도 떨어지지만 부정합 문제가 발생할 확률이 줄어든다.
격리 수준이 높아질수록 MySQL 서버의 처리 성능이 많이 떨어질 것으로 생각하지만, 사실 SERIALIZABLE 격리 수준이 아니라면 크게 성능의 개선이나 저하는 발생하지 않는다.

## 격리 수준에 따라 발생할 수 있는 문제점
세 가지 부정합의 문제점이 있는데, 격리 수준의 레벨에 따라 발생할 수도 있고 발생하지 않을 수도 있다.

| |DIRTY READ|NON-REPEATABLE READ|PHANTOM READ|
|---|---|---|---|
|READ UNCOMMITTED|발생|발생|발생|
|READ COMMITTED|없음|발생|발생|
|REPEATABLE READ|없음|없음|발생<br>(InnoDB는 없음)|
|SERIALIZABLE|없음|없음|없음|

### Dirty Read(더티 리드)
어떤 트랜잭션에서 처리한 작업이 완료되지 않았는데도 다른 트랜잭션에서 조회할 수 있는 현상이다. 
더티 리드 현상은 데이터가 나타났다가 사라졌다 하는 현상을 초래하므로 개발자와 사용자를 상당히 혼란스럽게 만들 것이다.

### Non-Repeatable Read(반복 불가능한 읽기)
하나의 트랜잭션 내에서 똑같은 SELECT 쿼리를 실행했을 때 읽어온 데이터(결과)가 다른 현상이다. 
Non-Repeatable Read 현상은 일반적인 웹 프로그램에서는 크게 문제되지 않을 수 있지만 하나의 트랜잭션에서 동일 데이터를 여러 번 읽고 변경하는 작업이 금전적인 처리와 연결되면 문제가 될 수도 있다.

### Phantom Read(팬텀 리드)
다른 트랜잭션에서 수행한 변경 작업에 의해 레코드가 보였다 안 보였다 하는 현상이다. 
InnoDB에서는 독특한 특성 때문에 REPEATABLE READ 격리 수준에서도 PHANTOM READ가 발생하지 않는다.

## READ UNCOMMITTED
READ UNCOMMITTED 격리 수준에서는 다음과 같이 각 트랜잭션에서의 변경 내용이 COMMIT이나 ROLLBACK 여부에 상관없이 다른 트랜잭션에서 조회할 수 있다.

![read uncommitted](https://user-images.githubusercontent.com/47477359/211150364-575000e9-5f92-48a7-af35-de90c430c1c5.png)

위에서 Transaction 1은 id가 2이고 name이 "짱아"인 사용자를 INSERT한다.
Transaction 1이 변경된 내용을 커밋하기도 전에 Transaction 2는 id = 2인 사용자를 검색하고 있다.
하지만 Transaction 2는 Trasaction 1이 INSERT한 사용자 정보를 커밋되지 않은 상태에서도 조회할 수 있다.
그런데 문제는 Transaction 1이 처리 도중 알 수 없는 문제가 발생해 INSERT된 내용을 롤백한다고 하더라도 여전히 Transaction 2는 "짱아"가 정상적인 사용자라고 생각하고 계속 처리할 것이다.

이처럼 더티 리드가 허용되는 격리 수준이 READ UNCOMMITTED다. 
더티 리드를 유발하는 READ UNCOMMITTED는 RDBMS 표준에서 트랜잭션의 격리 수준으로 인정하지 않을 정도로 정합성에 문제가 많은 격리 수준이다. 

## READ COMMITTED
READ COMMITTED 격리 수준에서는 다음과 같이 어떤 트랜잭션에서 데이터를 변경했더라도 COMMIT이 완료된 데이터만 다른 트랜잭션에서 조회할 수 있다.

![read committed](https://user-images.githubusercontent.com/47477359/211150362-613c1a96-9208-42e4-aa43-3b37f011e3c9.png)

위에서 Transaction 1은 id = 2인 사용자의 name을 "짱아"에서 "흰둥이"로 변경했는데, 이때 새로운 값인 "흰둥이"는 테이블에 즉시 기록되고 이전 값인 "짱아"는 언두 영역으로 백업된다. 
Transaction 1이 커밋을 수행하기 전에 Transaction 2가 id = 2인 사용자를 SELECT하면 조회된 결과의 name 칼럼의 값은 "흰둥이"가 아니라 "짱아"로 조회된다. 
여기서 Transaction 2의 SELECT 쿼리 결과는 테이블이 아니라 언두 영역에 백업된 레코드에서 가져온 것이다.

## REPEATABLE READ
REPEATABLE READ는 MySQL의 InnoDB 스토리지 엔진에서 기본으로 사용되는 격리 수준이다. 
InnoDB 스토리지 엔진은 트랜잭션이 ROLLBACK될 가능성에 대비해 변경되기 전 레코드를 언두(Undo) 공간에 백업해두고 실제 레코드 값을 변경하며, 이러한 변경 방식을 MVCC(Multi Version Concurrency Control)라고 한다. 

REPEATABLE READ 격리 수준에서는 MVCC를 위해 언두 영역에 백업된 이전 데이터를 이용해 동일 트랜잭션 내에서는 동일한 결과를 보여줄 수 있게 보장한다. 
모든 InnoDB의 트랜잭션은 고유한 트랜잭션 번호(순차적으로 증가하는 값)를 가지며, 언두 영역에 백업된 모든 레코드에는 변경을 발생시킨 트랜잭션의 번호가 포함돼 있다. 
REPEATABLE READ 격리 수준에서는 MVCC를 보장하기 위해 실행 중인 트랜잭션 가운데 가장 오래된 트랜잭션 번호보다 트랜잭션 번호가 앞선 언두 영역의 데이터는 삭제할 수가 없다. 
특정 트랜잭션 번호의 구간 내에서 백업된 언두 데이터가 보존돼야 한다.

![repeatable read](https://user-images.githubusercontent.com/47477359/211150367-851f3655-cece-4328-a962-571e19ac8f95.png)

위에서 이 시나리오가 실행되기 전에 테이블은 번호가 6인 트랜잭션에 의해 INSERT됐다고 가정한다. 
Transaction 12는 사용자 이름을 "흰둥이"로 변경하고 커밋을 수행했다. 
그런데 Transaction 10은 id = 2인 사용자를 Trasaction 12의 변경 전후 각각 한 번씩 SELECT했는데 결과는 항상 "짱아"라는 값을 가져온다. 
Transaction 10에서 실행되는 모든 SELECT 쿼리는 트랜잭션 번호가 10(자신의 트랜잭션 번호)보다 작은 트랜잭션 번호에서 변경한 것만 보게 된다.

## SERIALIZABLE
SERIALIZABLE 격리 수준에서는 특정 트랜잭션에서 읽고 쓰는 레코드를 다른 트랜잭션에서는 절대 접근할 수 없다. 
가장 높은 데이터 정합성을 갖는 반면에, 성능은 가장 떨어진다. 
단순한 SELECT 쿼리가 실행되더라도, 락이 걸려 다른 트랜잭션에서 데이터에 접근할 수 없게된다.
