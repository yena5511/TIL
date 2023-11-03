## Collection

자바에서 여러 원소를 담을 수 있는 자료구저
(데이터의 집합, 그룸을 의미)

![](https://miro.medium.com/v2/resize:fit:828/0*uunOsk05Y0QEA6nP)

- Map은 Collection 인터페이스를 상속받지는 않지만 Collection으로 분류된다.

## MAP

- Map은 인터페이스이다.
- Key와 Value의 쌍으로 이루어진 데이터의 집합니다.
-  맵(Map)의 가장 큰 특징이라면 key로 value를 얻어낸다는 점이다. 
- 인터페이스를 구현하기 위해선 구현 클래스가 필요하다.
- Map의 구현 클래스로는 Hashtable, HashMap, SortedMap 등이 있다.

#### 특징
- 저장 순서를 유지하지 않는다.
- 키는 중복을 허용하지 않는다.(값의 중복은 허용)

## HashMap

- 해시맵은 이름 그대로 해싱(Hashing)된 맵(Map)이다.
- HashMap은 Map인터페이스를 상속하고 있기에 Map의 성질을 그대로 가지고 있다.
- Map은 Key와 Value로 구성된 Entry 객체를 저장하는 구조를 가지고 있는 자료구조이다.
- 여기서 Key와 Value는 모두 객체로 이루어저 있다.
- Key와 Value는 한 쌍으로 Key롤 식별하고 Value에 사용할 값을 넣는 식이다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdTmPfZ%2Fbtrh2iRDKCr%2FuDluAR6dZ93NzKaXRFKXlk%2Fimg.jpg)

#### 주요 메서드

|메서드|설명|
|:---|:---|
|Map.put(key, value);|Map 안에 값 넣기|
|Map.get(key);|Map 안의 값 가져오기|
|Map.size();|	Map 크기 확인|
|Map.replace(key, value);|	Map 안의 내용 변경하기|
|Map.containsKey(key);
Map.containsValue(value);|Map 안에 특정 Key, Value 들었는지 확인|
|Map.isEmpty();|Map의 크기가 0인지 확인|
|Map.remove(key);|Map 안의 내용 삭제|
|Map.getOrDefault(key, default);|Key가 있으면 Value를 가져오고 없으면 default값 가져오기|

```java
import java.util.HashMap;
import java.util.Map;

public class Main {

	public static void main(String[] args) {
		Map<String,Integer> map=new HashMap();
		map.put("A", 1000);
		map.put("B", 2000);
		map.put("C", 3000);
		map.put("C", 4000);
		System.out.println(map);
		System.out.println(map.get("A"));
		System.out.println(map.get("B"));
		System.out.println(map.get("C"));
	}

}
```

`결과`
```java
{A=1000, B=2000, C=4000}
1000
2000
4000
```
