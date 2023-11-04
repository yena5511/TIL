## 스프링 순환 참조(Circular Reference)

서로 다른 빈들이 서로 참조를 맞물리게 주입되면서 생기는 현상
beanA에서 beanB를 참조하게 되는데 beanB에서도 beanA를 참조해야 하는 경우 순환 참조 문제가 생긴다

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F7c0a1e25-631f-4d85-90f6-b44beae617ba%2FUntitled.png&blockId=5d7668a1-1021-49ba-a2ae-1191ef5982ae)

## 순환 참조가 발생하는 경우 3가지

순환참조는 맞물리는 `DI(Dependency Injection)`상황에서 스프링이 어느 스프링 빈을 먼저 생성할지 결정하기 못하기 때문이다.
그렇기 때문에 이 순환참조 문제도 DI를 하는 방법 3가지 상황에서 발생할 수 있는데, 생성자 주입 방식, 필드 주입 방식, Setter 주입 방식이 있고, 이 중에서 생성자 주입 방식은 순환참조 문제가 다르게 발생한다.

#### 생성자 주입 방식

```java
@Component
public class BeanA {
	private BeanB beanB;

	public void BeanA(BeanB beanB){
		this.beanB = beanB;
	}
}

@Component
public class BeanB {
	private BeanA beanA;

	public void BeanB(BeanA beanA){
		this.beanA = beanA;
	}
}
```

애플리케이션을 구동하면 이제 스프링 컨테이너(IOC)는 BeanA 빈을 생성하기위해 BeanB를 주입해줘야하기 때문에 BeanB를 찾을 것이다.
근데 BeanB를 생성하려 하니 BeanA가 필요해서 BeanA를 주입하기위해 BeanA를 찾게되면서 무한 반복이 생기게 된다. 

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F971879b6-6a63-4a86-9920-aaf74f5d9b30%2FUntitled.png&blockId=01ed0b99-c13b-40a4-9232-c89a90b29d5a)

#### 필드, Setter 주입 방식

필드와 Setter 주입 방식은 순환 참조가 발생하는 시점이 생성자 주입 방식과는 다르다.

```java
@Component
@Slf4j
public class BeanA {
	@Autowired
	private BeanB beanB;

	public void run(){
		beanB.run();
	}

	public void call(){
		log.info("called BeanA");
	}
}

@Component
@Slf4j
public class BeanB {
	@Autowired
	private BeanA beanA;

	public void run(){
		log.info("Called BeanB");
		beanA.call();
	}
}
```
Field Injection 방식

```java
@Component
@Slf4j
public class BeanA {
	private BeanB beanB;

	@Autowired
	public void setBeanB(BeanB beanB){
		this.beanB = beanB;
	}

	public void run(){
		beanB.run();
	}

	public void call(){
		log.info("called BeanA");
	}
}

@Component
@Slf4j
public class BeanB {
	private BeanA beanA;

	@Autowired
	public void setBeanA(BeanA beanA){
		this.beanA = beanA;
	}

	public void run(){
		log.info("Called BeanB");
		beanA.call();
	}
}
```
Setter Injection 방식

이 두가지 DI 방식은 순환참조 문제가 애플리케이션 구동 당시에는 발생하지 않는다.

이 두가지 방식은 애플리케이션 구동 시점에서는 필요한 의존성이 없을 경우에는 NULL 상태로 유지하도 실제로 사용하는 시점에 주입을 하기 때문이다.
그렇기에 위 두사지 방식은 모두 순환참조를 일으킬 수 있는 메서드를 호출하는 시점에서 순환 참조 문제가 발생할 것이다.

## 해결방법

순환을 끊음으로써 문제를 해결해야하는데, 스프링에서 `@Lazy`하는 어노테이션을 통해 끊을 수 있도록 한다.

```java
@Component
public class BeanA {
	private BeanB beanB;

	public void BeanA(BeanB beanB){
		this.beanB = beanB;
	}
}

@Component
public class BeanB {
	private BeanA beanA;

	public void BeanB(@Lazy BeanA beanA){
		this.beanA = beanA;
	}
}
```