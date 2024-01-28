---
tags:
  - 학습정리
  - Spring
  - JPA
  - Spring_Data_JPA
  - Hibernate
---
>[!note] @DynamicUpdate 란
> 실제 값이 변경된 Column 으로만 update 쿼리를 만드는 기능

- Spring Data JPA 를 Hibernate 와 함께 쓸 때 사용할 수 있는 기능이다.
- JPA Entity 에 적용될 수 있는 Class 레벨의 어노테이션이다.

- 애플리케이션을 시작하면 Hibernate 는 모든 Entity 들의 CRUD 작업을 위한 SQL문을 생성한다.
- 이 SQL 문들은 한번 생성된 후, 성능 향상을 위해 메모리에 캐시된다.

- 생성된 update SQL 문은 Entity 의 모든 Column 을 포함한다.
- 여기서 특정 Column 의 값만 바꾸려고 Update SQL 을 사용하면 변경되지 않은 나머지 Column 에는 기존 값들이 들어가게 된다.

### @DynamicUpdate 추가 전

```sql
select a1_0.id,a1_0.active,a1_0.name,a1_0.type from account a1_0 where a1_0.id=?
```

### @DynamicUpdate 추가 후

```sql
update account set name=? where id=?
```

### 원리

- 캐시된 SQL 문을 사용하지 않고 새로 Update SQL 문을 생성한다.
- 따라서 성능 오버헤드가 존재하기 때문에 필요한 경우에만 사용해야 한다.

## 쓸 타이밍

- Table 에 Column 이 많은데 그중 몇개의 Column 만 반복해서 업데이트 될 경우
- version lesss 한 optimistic locking 일 경우

# Reference

- https://www.baeldung.com/spring-data-jpa-dynamicupdate
