# JPA

- java ORM 기술 API 표준 명세
  - ORM: 객체와 관계형 데이터베이스를 매핑하는 방법, ORM 프레임워크는 객체와 테이블을 매핑해서 패러다임의 불일치 문제를 해결한다.
  - 자바 플랫폼 SE와 자바 플랫폼 EE를 사용하는 응용프로그램에서 관계형 데이터베이스의 관리를 표현하는 자바 API이다.
  - JPA는 ORM을 사용하기 위한 표준 인터페이스를 모아둔 것이다.
- JPA 구성 요소
  - javax.persistance 패키지로 정의된 API
  - JPQL(Java Persistence Query Language)
  - 객체/관계 메타데이터
- 사용자가 원하는 JPA 구현체를 선택해서 사용할 수 있다.
  - JPA의 대표적인 구현체로는 Hibernate, EclipseLink, DataNucleus, OpenJPA, TopLink Essentials 등이 있다.
  - 구현체들을 ORM Framework라고 부른다.

## 영속성 컨텍스트
- 엔티티를 영구 저장하는 환경
- 엔티티 매니저를 생성하면 영속성 컨텍스트가 자동으로 생성되고, Entity Manager를 이용해서 영속성 컨텍스트에 엔티티를 보관/관리한다.
- 비영속: 영속성 컨텍스트와 전혀 무관한 상태로 순수한 객체의 상태 (처음 객체가 생성되면 비영속 상태)
- 영속: 영속성 컨텍스트에 저장된 상태
- 준영속: 영속성 컨텍스트에서 관리하다 영속성 컨텍스에서 분리된 상태, 준영속 상태는 영속 상태 였던 적이 있기 때문에 @Id 값을 가지고 있다. 하지만, 비영속은 순수한 객체의 상태이기 때문에 @Id 값을 안 가지고 있는 경우가 존재할 수 있다.
- 삭제: 삭제된 상태

### 1차 캐시
- 영속성 컨택스트 내부에는 1차 캐시라고 불리는 캐시를 가지고 있다. 영속상태의 엔티티는 모두 1차 캐시에 저장되고, 1차 캐시는 @Id를 키로 가지고 있는 Map이 존재한다. 1차 캐시를 이용하면 데이터의 결과를 빠르게 얻을 수 있는 장점을 가지고 있다. member 엔티티를 조회할 때, 바로 데이터베이스에서 직접 조회하는 것이 아니고, 우선적으로 1차 캐시에 member 엔티티가 있는지 @Id값으로 확인을 한 후에 엔티티가 있을 떄는 1차 캐시에서 엔티티가 없을 때는 데이터베이스에서 조회를 한 후, 1차 캐시에 저장하고, 1차 캐시에서 엔티티를 전달한다. 
- 1차캐시를 우선으로 확인하고, 데이터베이스를 조회한다.

### 동일성 보장
- 엔티티를 조회할 때, 1차 캐시에서 모든 엔티티를 가져온다. 모든 엔티티는 최종적으로 1차 캐시에서 가져오므로 식별자가 동일한 엔티티는 동일성 보장된다.

### 트랜잭션을 지원하는 쓰기지연
- 트랜잭션은 데이터베이스에서 하나의 작업단위를 의미한다. 영속성 컨텍스트는 삽입/삭제/수정과 같은 DML이 발생했을 때, 바로 데이터베이스에 반영하지 않고, 트랜잭션을 커밋할 때, 모아둔 쿼리들을 한번에 데이터베이스 반영을 한다. 이것이 쓰기 지연이다. 쿼리들은 영속성 컨텍스트의 쓰기지연SQL 저장소에 저장된다. 커밋을 실행 하면, flush -> commit 순서로 데이터베이스에 작업한다. flush는 쓰기지연SQL 저장소 쿼리들을 데이터베이스에 저장하는 것이고, commit은 데이터베이스에 쿼리들을 반영하는 것을 의미한다. 
- 쓰기지연을 하면, 데이터베이스에 접근하는 횟수가 줄기 때문에 성능면에서 뛰어나다.

### 변경 감지
- 영속성 컨텍스트의 1차캐시에는 스냅샷을 통해 엔티티의 변경을 감지한다. 쿼리를 통해 데이터를 조작하는 방법외에도 영속상태의 엔티티의 상태를 변경하는 방법으로도 데이터베이스의 데이터를 변경할 수 있다. 변경감지는 아래와 같은 순서로 동작한다.
````
a. 트랜잭션을 커밋하면, flush 가 호출되고, 엔티티와 스냅샷을 비교해서 변경된 엔티티를 찾는다.
b. 변경된 엔티티가 존재하면, update 쿼리를 생성해서 쓰기지연SQL 저장소에 저장한다.
c. 쓰기지연SQL 저장소에 생성된 쿼리들을 데이터베이스에 flush하고 commit 한다.
````
- 변경감지는 오직 영속 상태의 엔티티에만 적용이 된다. 비영속과 준영속은 해당되지 않는다. JPA에서 엔티티의 특정 필드가 변경되었을 때, 특정 필드만 update 하는 쿼리를 만드는 것이 아닌, 모든 필드를 업데이트 하는 쿼리를 만든다.

## 동작 과정
- 저장
  - jpa의 경우 트랜잭션 실행 단위안에서 동작한다. 객체를 생성하고 엔티티 매니저에 의해서 생성된 객체를 영속성 컨텍스트에 등록한다. 이때 해당 객체는 영속상태가 된다 즉 1차 캐시에 저장된다. 또한 insert 쿼리는 쓰기지연 SQL저장소에 저장이된다. 트랜잭션이 끝나는 시점에 쓰기 지연 SQL 저장소에 있는 쿼리문들이 flush가 되고 트랜잭션 커밋이 된다.

- 수정
  - 우선 수정할 엔티티를 찾는다. 이때 1차 캐시에 올라가고 영속상태가 된다. 즉 영속성 컨텍스트가 관리하는 상태가 된다. 그리고 setter으로 데이터를 수정한다. 이때 JPA는 데이터베이스 트랜잭션 커밋 시점에 내부적으로 flush가 된다. 영속성 컨텍스트 기능 중 변경감지 기능이 있는데, 이것이 현재 1차 캐시의 엔티티와 최소 1차 캐시에 등록된 상태의 스냅샷과 비교해서 변경내역을 확인하고 update 쿼리를 쓰기지연 저장소에 저장하고 트랜잭션 커밋 끝나기 전에 해당 쿼리를 날리고 커밋을 완료한다.

## Cascade
- 영속성 전이는 자신의 entity를 영속시킬 때, 자신과 연관된 entity를 같이 영속시키는 것이다.
- cascade 범위는 개인이 소유하는 entity일 때, 사용하는 것을 권장한다.
  - 게시판, 첨부 파일: 첨부 파일은 게시판 entity만 참조하기 때문에 개인 소유

````
@Entity
public class Parent {

    @Id 
    @GeneratedValue
    private Long id;
    private String name;

    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL)
    private List<Child> childs = new ArrayList<>();

    public void addChild(Child child){
        childs.add(child);
        child.setParent(this);
    }
}
````
````
@Entity
public class Child {

    @Id 
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne(fetch = FetchType.LAZY)
    private Parent parent;
}
````
````
Parent parent = new Parent();
parent.setName("parent1");

Child child1 = new Child();
child1.setName("child1");
Child child2 = new Child();
child2.setName("child2");

parent.addChild(child1);
parent.addChild(child2);

em.persist(parent);

tx.commit();
````

## orphanRemoval
- orphanRemoval = true
  -  부모 entity와 연관관계가 끊어진 자식 entity를 삭제
  -  부모 entity에서 해당 컬렉션의 자식 entity 제거하는 경우 자식 entity가 제거되도록 표시
````
@Entity
public class Parent {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Child> childs = new ArrayList<>();
}
````
````
Parent parent = em.find(Parent.class, parent.getId());
parent.getChilds().remove(0); // delete query
````

## Lock
- JPA를 사용하면서 READ COMMITTED 이상의 격리 수준이 필요할 때 비관적 락, 낙관적 락 선택
- 낙관적 락(Optimistic Lock)
  - Transaction 충돌이 발생하지 않는다는 가정
  - DB 가 제공하는 락 기능을 사용하지 않음. JPA 가 제공하는 버전 관리 기능 사용
  - Transaction을 Commit 전까지 Transaction의 충돌을 알 수 없음
  - @Version
  - update를 하기 전에 version 체크 -> version 이 맞지 않으면 OptimisticLockException 예외
- 비관적 락(Pessimistic Lock)
  - Transaction 충돌이 발생한다고 가정
  - DB 가 제공하는 락 기능을 사용
  - Select for Update
  - 데이터 수정 시 즉시 Transaction 충돌을 알 수 있음


## N+1 문제
- 하나의 쿼리를 수행하는데 N개의 쿼리가 더 수행된다는 의미이다.
- Member 객체와 Team 객체가 연관되어 있다고 하자. 10명의 Member를 조회할 때 지연 로딩으로 가져오면 Team은 프록시 객체를 사용하고 직접 DB에서 조회하지는 않는다. 근데 즉시로딩이라면 가져올 때 무조건 값이 실제 데이터로 채워져 있어야 해서 Team까지 DB에서 조회해야 한다. 이때 10명의 Member를 조회하기 위해 select * from ~이라는 식의 쿼리문 1개를 사용해서 10개의 데이터를 조회했는데, 이 10개의 멤버에 대해 각각 select * from Team where member.id = ~~ 의 식으로 쿼리문이 N개(여기선 10개겠지) 나가서 매우 비효율적인 상황이 된다. 

### 해결방안
- Batch Size
  - @BatchSize 어노테이션을 활용하면 설정한 size만큼의 데이터를 미리 로딩한다. 즉 연관 엔티티를 조회할 때 size만큼 where in 쿼리를 통해 조회하고, size를 넘어갔을 때 추가로 where in 쿼리를 수행한다. 하지만 이 방법도 결국 정해진 batch size에 따라 반복적인 쿼리가 수행된다는 문제가 있다.
 
- 페치 조인 사용
  - @Query("select m from Member m left join fetch m.orders")와 같이 적으면 fetch를 사용해서 조인쿼리를 수행한다. fetch 키워드는 연관 객체나 컬렉션을 한 번에 같이 조회하게 한다. 즉, 페치 조인을 사용하면 연관 엔티티는 프록시가 아닌 실제 엔티티를 조회하게 되고 이로써 연관 객체까지 한번의 쿼리로 다 가져올 수 있다. N번 실행하지 않게 된다.
  - 일반 조인과 페치 조인의 차이: 일반 조인은 select m from Member m join m.orders이고, 페치 조인은 select m from Member m join fetch m.orders 이다. 일반 조인으로 현 상황을 적용하면 N+1문제가 난다. 페치 조인만 문제 없이 가져와진다.

## JPA Entity primitive vs wrapper
- nullable 가능한 타입 권장
````
Cat has a property called id. This property maps to the primary key column of a database table. The property might have been called anything, and its type might have been any primitive type, any primitive "wrapper" type, java.lang.String or java.util.Date. If your legacy database table has composite keys, you can use a user-defined class with properties of these types (see the section on composite identifiers later in the chapter.)

The identifier property is strictly optional. You can leave them off and let Hibernate keep track of object identifiers internally. We do not recommend this, however.

We recommend that you declare consistently-named identifier properties on persistent classes and that you use a nullable (i.e., non-primitive) type.
````

## Saving Entities
````
By default Spring Data JPA inspects the identifier property of the given entity. If the identifier property is null, then the entity will be assumed as new, otherwise as not new.
````
-> entity has an ID field not null, Spring will make Hibernate do an update (and so a SELECT before).
````

````
- Entity implements Persistable which will make you implement the method "isNew".

## @DynamicUpdate
- 업데이트 된 컬럼만 변경
- Hibernate는 애플리케이션을 처음 로드할 때, entity를 모두 스캔하여 업데이트할 쿼리를 캐시해놓고 사용한다.
- DynamicUpdate를 사용하면 캐싱을 하지 않고 변경된 컬럼에 맞는 새로운 동적 쿼리를 만든다.
- 상황에 맞게 사용

## delete
- delteAll
  -  내부에서 findAll 메서드를 호출하여 전체 데이터를 조회
  - 한 건씩 delete 처리
  - 하나의 트랜잭션에서 처리

````
/*
 * (non-Javadoc)
 * @see org.springframework.data.repository.Repository#deleteAll()
 */
@Override
@Transactional
public void deleteAll() {

	for (T element : findAll()) {
		delete(element);
	}
}
````
- deleteAllInBatch
  - delete from ${table} 처리
````
/*
 * (non-Javadoc)
 * @see org.springframework.data.jpa.repository.JpaRepository#deleteInBatch(java.lang.Iterable)
 */
@Override
@Transactional
public void deleteAllInBatch(Iterable<T> entities) {

	Assert.notNull(entities, "Entities must not be null!");

	if (!entities.iterator().hasNext()) {
		return;
	}

	applyAndBind(getQueryString(DELETE_ALL_QUERY_STRING, entityInformation.getEntityName()), entities, em)
			.executeUpdate();
}
````

- delete query
````
@Transactional
@Modifying
@Query("delete from User u where u.id in :ids")
void deleteAllById(@Param("ids") List<Long> ids);
````
- @Modifying: @Query Annotation으로 작성 된 변경, 삭제 쿼리 메서드를 사용할 때 / 데이터에 변경이 일어나는 INSERT, UPDATE, DELETE, DDL 에서 사용
  - JPA Entity LifeCycle을 무시하고 쿼리가 실행 되어, 영속성 콘텍스트 관리에 주의
  - clearAutomatically: @Modifying 쿼리 메서드 실행 후, 영속성 컨텍스트를 clear 할 것인지를 지정

## QueryDsl
### fetchResults() deprecated
````
fetchResults requires a count query to be computed. 
In querydsl-sql, this is done by wrapping the query in a subquery, like so: SELECT COUNT(*) FROM (<original query>).
Unfortunately, JPQL - the query language of JPA - does not allow queries to project from subqueries. 
As a result there isn't a universal way to express count queries in JPQL. 
Historically QueryDSL attempts at producing a modified query to compute the number of results instead. 
However, this approach only works for simple queries.
Specifically queries with multiple group by clauses and queries with a having clause turn out to be problematic.
This is because COUNT(DISTINCT a, b, c), while valid SQL in most dialects, is not valid JPQL. 
Furthermore, a having clause may refer select elements or aggregate functions and therefore cannot be emulated by moving the predicate to the where clause instead.
In order to support fetchResults for queries with multiple group by elements or a having clause, we generate the count in memory instead.
This means that the method simply falls back to returning the size of fetch(). 
For large result sets this may come at a severe performance penalty.
For very specific domain models where fetchResults() has to be used in conjunction with complex queries containing multiple group by elements and/or a having clause, we recommend using the Blaze-Persistence  integration for QueryDSL.
Among other advanced query features, Blaze-Persistence makes it possible to select from subqueries in JPQL.
As a result the BlazeJPAQuery provided with the integration, implements fetchResults properly and always executes a proper count query.
Mind that for any scenario where the count is not strictly needed separately, we recommend to use fetch() instead.
````
````
public Page<User> findUserWithPaging(Pageable pageable) {

	List<User> content = queryFactory
			.selectFrom(user)
			.where(user.username.like("user_"))
			.offset(pageable.getOffset()) // offset
			.limit(pageable.getPageSize()) // limit
			.fetch(); 
	return new PageImpl<>(content, pageable, content.size()); 
}
````

### Specify alias on join clause
````
Cat cat = new Cat("C");
Owner owner = new Owner("O");
query.from(cats).join(owners).on(...)
````
https://docs.jboss.org/hibernate/orm/3.3/reference/en/html/persistent-classes.html#persistent-classes-pojo-identifier
https://stackoverflow.com/questions/51642979/boxed-vs-primitive-type-as-entity-id
