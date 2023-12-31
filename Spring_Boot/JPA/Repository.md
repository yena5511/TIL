## 핵심개념

Spring Data 저장소 추상화의 중앙 인터페이스는 Repository이다.
관리할 도메인 클래스와 도메인 클래그의 id 유형을 유형 인수로 사용한다.
이 인터페이스는 주로 작업할 유형을 캡처하고 이 인터페이스를 확장하는 인터페이스를 검색하는 데 도움이 되는 마커 인터페이스 역할을 한다.
CrudRepository관리되는 엔터티 클래스에 대한 정교한 CRUD 기능을 제공한다.

`예제 CrudRepository상호 작용`
```java
public interface CrudRepository<T, ID extends Serializable>
    extends Repository<T, ID> {
                                                                                                                       (1)
    <S extends T> S save(S entity);
                                                                                                                       (2)
    T findOne(ID primaryKey);
                                                                                                                       (3)
    Iterable<T> findAll();

    Long count();
                                                                                                                       (4)
    void delete(T entity);
                                                                                                                   (5)
    boolean exists(ID primaryKey);
                                                                                                                       (6)
    // … more functionality omitted.
```
1. 저장된 엔티티를 저장한다.
2. 주어진 ID로 식별되는 엔티티를 반환한다.
3. 모든 엔티티를 반환한다.
4. 엔티티 수를 반환한다.
5. 저장된 엔티티를 삭제한다.
6. 주어진 ID를 가진 엔티티가 존재하는지 여부를 나타낸다.

