쿼리 단에서 페이징 처리를 할 수 있는 방법들에 대해 알아보자!

## 1. offset, limit

sql문에 offset, limit 구문을 넣어 페이징 처리한다.

```sql
select id, title, content, author, created_at, updated_at
from board
order by created_at
limit 20  -- 한 페이지에 보여줄 개수
offset 20; -- 시작 row 번호
```

이 쿼리문의 동작 순서는 아래와 같다.

```
from board -> select ... -> order by -> offset, limit
```

**offset까지 도달하기 위해, 조회 대상이 아닌 row들을 모두 lookup 하기 때문**에 성능 저하가 발생한다. 그렇다면 offset과 limit을 쓰지 않고 어떻게 페이징을 구현할 수 있을까?

## 2. noOffset

offset 대신에 pk의 인덱스를 활용한다.

```sql
select id, title, content, author, created_at, updated_at
from board
where id > 25 -- 마지막으로 조회한 pk 값
order by created_at
limit 20;
```

위 경우, PK인 id의 인덱스를 타고 조회할 행의 시작 지점을 찾아내기 때문에 낭비가 없다. 다만, 다음 데이터를 가져오기 위해 해당 쿼리로 조회된 값들의 마지막 pk를 기억해야 한다( _다음 데이터를 가져오는 url을 제공하는 방식으로 API를 구현할 수 있다_ ).

## 3. 커버링 인덱스

> < 1 2 3 4 5 >

위처럼, 특정 페이지에 바로 접근할 수 있게 하면서 쿼리의 성능을 개선하는 방법이 커버링 인덱스이다.

```sql
select id, title, content, author, created_at, updated_at
from (select id
      from board
      order by id desc
      limit 20
      offset 20) b1 join board b2 on b1.id = b2.id;
```

**인덱스가 걸린 컬럼에서 모든 작업이 일어나도록** 인라인 쿼리를 작성한다. 이렇게 인덱스를 타고 조회된 행들의 id(pk)로 다시 select 절에 명시된 컬럼들의 값을 가져온다(이 과정에서 또 pk의 인덱스를 탄다).

커버링 인덱스 쿼리를 작성할 때는 explain으로 실행계획을 조회하여 인덱스를 잘 타고 있는지 확인하도록 하자.
