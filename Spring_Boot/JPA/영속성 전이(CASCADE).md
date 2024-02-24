## 영속성 전이(CASCADE)

부모 엔티티가 영속화될 때 자식 엔티티도 같이 영속화 되고, 부모 엔티티가 삭제 될 때 자식 엔티티도 삭제되는 등 특정 엔티티를 영속 상태로 만들 때 연관된 엔티티도 함꼐 영속 상태로 전이되는 것 이다.

```java
@Setter
@Getter
@Entity
public class Parent {

    @Id @GeneratedValue
    @Column(name = "PARENT_ID")
    private Long id;

    @OneToMany(mappedBy = "parent")
    private List<Child> childList = new ArrayList<>();
}
```

```java
@Getter
@Setter
@Entity
public class Child {

    @Id @GeneratedValue
    @Column(name = "CHILD_ID")
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    private Parent parent;
}
```

#### 종류

|operations|설명|
|:---|:---|
|CascadeType.ALL|모든 Cascade를 적용|
|CascadeType.PERSIST|엔티티를 영속화할 때, 연관된 엔티티도 함께 유지|
|CascadeType.MERGE|엔티티 상태를 병합(Merge)할 때, 연관된 엔티티도 모두 병합|
|CascadeType.REMOVE|엔티티를 제거할 때, 연관된 엔티티도 모두 제거|
|CascadeType.DETACH|부모 엔티티를 detach() 수행하면, 연관 엔티티도 detach()상태가 되어 변경 사항 반영 X|
|CascadeType.REFRESH|상위 엔티티를 새로고침(Refresh)할 때, 연관된 엔티티도 모두 새로고침|

## 고아객체

JPA는 부모 엔티티와 연관관계가 끊어진 자식 엔티티를 자동으로 삭제하는 기능을 제공하는데 이것을 고아 객체 제거라고 한다.
고아 객체 제거 기능은 영속성 컨텍스트를 플러시할 때 적용되므로 플러시 시점에 DELETE SQL이 실행된다.

##### 설정 방법

```java
@Entity
@Getter @Setter
public class Parent {
    @Id @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "parent", orphanRemoval = true)
    private List<Child> childList = new ArrayList<>();

    public void addChild(Child child) {
        childList.add(child);
        child.setParent(this);
    }
}

```

고아 객체 제거 기능을 활성화하기 위해 컬렉션에 orphanRemoval = true를 설정한다.
이제 컬렉션에서 제거한 엔티티는 자동으로 삭제된다.

#### 주의점

만약 삭제한 엔티티를 다른 곳에서도 참조한다면 문제가 발생할 수 있다. 
이런 이유로 orphanRemovel은 @OneToOne, @OneToMany에만 사용할 수 있다.