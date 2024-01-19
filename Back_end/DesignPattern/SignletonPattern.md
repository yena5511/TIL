## 싱글톤 패턴 (Signleton Pattern)

- 소프트웨어 디자인 패턴 중 하나로 클래스의 인스턴스가 오직 하나만 생성 되도록 보장하는 패턴으로 전역 변수를 사용하지 않고도 단일 인스턴스에 접근 할 수 있도록 한다.
- 에플리케이션이 실행될 때 최소 한 번만 메모리를 할당하고 여러 차례 호출이 되더라도 실제로 생성되는 객체는 하나이고 최초 생성된 이후 호출된 생성자는 최초 생성한 객체를 리턴한다.
- 이를 통해 여러 곳에서 공유하여 사용할 수 있으므로 메모리 절약 및 자원 관리 등의 장점을 얻을 수 있다.
- 또한 이를 통해 생성된 리소스에 대한 중복 액세스를 피하고 객체간의 일관성을 유지하기 위해 사용되며 주로 로깅, 드라이버 개체, 캐싱 및 스레드 폴, 데이터베이스 연결에 사용된다.

#### 싱글턴 패턴의 사용예시

- 데이터 베이스 연결
    - 데이터 베이스 연결은 일반적으로 오버헤드가 큰 작업이므로, 매번 연결 객체를 생성하는 것을 비효율적이다.
    이런 경우 싱클턴 패턴을 사용하여 하나의 연결 객체를 생성하고, 다른 클래스에서 이 객체를 공유하여 사용할 수 있다.
    - 여려 객체가 공유하는 단일 DB연결은 모든 객체에 대해 별도의 DB연결을 생성하는 데 비용이 많이 들 수 있다.
    그렇기에 단일 인스턴스를  사용하는 싱글턴 패턴을 이용한다.
- 로깅 시스템
    - 애플리케이션 전반에서 로그를 기록하고 관리하기 위해 사용되는데, 이때 싱글턴 패턴을 사용하면 여러 곳에서 동시에 로깅 시스템에 접근하여 로그를 기록 할 수 있다.
    - 여러 스레드에서 동시에 로그를 작성하더라도 싱글턴 패턴을 사용하면 하나의 로깅 인스턴스를 통해 동시에 로그를 기록할 수 있다.

#### 특징

|특징|설명|
|:---|:---|
|인스턴스의 유일성 보장|	싱글턴 패턴은 클래스의 인스턴스가 오직 하나만 생성되도록 보장한다. 이를 통해 메모리 낭비를 방지하고, 여러 곳에서 일관된 방식으로 인스턴스에 접근할 수 있다.|
|안전한 인스턴스 생성|다중 스레드 환경에서 싱글턴 패턴을 사용하면 인스턴스 생성이 안전하게 이루어 진다. 여러 스레드에서 동시에 인스턴스를 생성하려는 경우, 동기화 메커니즘을 사용하여 하나의 인스턴스만 생성되도록 보장할 수 있다.|
|전역적인 접근성|	싱글턴 패턴을 사용하면 어디서든 쉽게 인스턴스에 접근할 수 있다. 전역 변수로 선언될 수 있어 다른 객체들이 인스턴스에 접근할 수 있고, 필요한 경우 인스턴스를 공유하여 사용할 수 있다.|

#### 싱글톤 패턴 종류

|싱글톤 종류|설명|
|:---|:---|
|지연 초기화|객체가 처음으로 접근될 때만 초기화되는 방식이다.|
|이론 초기화|객체가 생성되자마자 즉시 초기화되는 방식이다.|
|스레드 안전 지연 초기화|다중 스레드 환경에서 경합 조건을 방지하여 지연 초기화를 스레드 안전하게 보장하는 방식이다.|
|정적 블록 초기화|정적 블록 코드 내에서 해당 객체를 초기화하는 방식이다.|
|더블 체크 락킹| 더블 체크 락킹을 사용하여 성능을 최적화하여 스레드 안전한 자연 초기화 메커니즘을 제공하는 방식이다.|
|빌 퓨 솔류선|스레드 안전한 방식으로 지연 초기화를 달성하기 위해 정적 내부 도우미 클래스의 활용하는 방식이다.|
|Enum 활용방식|열거형을 사용하여 싱글톤 인스턴스를 정의하는 방식이다.|

싱글턴 패턴의 종류별 변화

- 이론 초기화 -> (개선) -> 정적 블록 초기화
- 지연 초기화 방식 -> (개선) -> 스레드 안전 지연 초기화
- 스레드 안전 지연 초기화 -> (멀티스레드 환경)-> 더블 체크 락킹
- 지연 초기화 방식 + 스레드 안전 지연 초기화 = 빌 퓨 솔류션
- 최종 간략 방식 -> Enum 활용 방식

```java
package singleton.after;

public class Settings {

  private boolean darkMode = false;
  private int fontSize = 13;

  public boolean getDarkMode () { return darkMode; }
  public int getFontSize () { return fontSize; }

  public void setDarkMode (boolean _darkMode) { 
    darkMode = _darkMode; }
  public void setFontSize (int _fontSize) { 
    fontSize = _fontSize; }
}
```
- settings 란 클래스에서 다크모드 여부와 폰트 사이즈를 설정허고 접근 

```java
package singleton.after;

public class FirstPage {
  private Settings settings = Settings.getSettings();

  public void setAndPrintSettings () {
    settings.setDarkMode(true);
    settings.setFontSize(15);
    System.out.println(settings.getDarkMode() + " " + settings.getFontSize());
  }
}
```

```java
package singleton.after;

public class SecondPage {
  private Settings settings = Settings.getSettings();

  public void printSettings () {
    System.out.println(settings.getDarkMode() + " " + settings.getFontSize());
  }
}
```
- 페이지마다 다른 클래스를 만든다
- firstPage에서 settings 겍체의 설정들을 변경하고 그 내용을 출력
- secondPage에도 setting을 출력하는 함수를 만들고 메인에서 차례로 실행
-> firstPage에서 변경한 내용들이 secondPage에는 적용되지 않음

*settings 클래스 변경*
```java
package singleton.after;

public class Settings {

  private Settings () {};
  private static Settings settings = null;

  public static Settings getSettings () {
    if (settings == null) {
      settings = new Settings();
    }
    return settings;
  }

  private boolean darkMode = false;
  private int fontSize = 13;

  public boolean getDarkMode () { return darkMode; }
  public int getFontSize () { return fontSize; }

  public void setDarkMode (boolean _darkMode) { 
    darkMode = _darkMode; }
  public void setFontSize (int _fontSize) { 
    fontSize = _fontSize; }
}
```

- static이 아닌 변수나 메소드들은 객체가 생성될 때마다 메모리의 공간을 새로 처지
- static으로 선언될 것들은 객체가 얼마나 만들어지든 메모리에 지정된 공간을 딱 하나씩만 존재

- 이 방법을 사용하면 멀티쓰레드에 안전하고, Lazy loading을 지원하며, serialization에도 안전한 싱글톤을 만들 수 있다
