## Connection Pool

#### 데이터베이스 커넥션

우리가 개발하는 웹 애플리케이션과 데이터 베이스는 서로 다른 시스템이다.
데이터 베이스드라이버를 사용하여 데이터베이스를 연결해야한다.

#### 데이터베이스 커넥션 풀 (DBCP)

![](https://hudi.blog/static/1bda5b43f837f4e11d0d6934aa003aa0/9d567/database-connection-pool.png)

- 웹 컨테이너가 실행되면서 일정량의 Connection 객체를 만들어서 pool저장 했다가, 클라이언트 요청이 오면 Connection 객체를 빌려주고 해당 객체의 임무가 완료되면 다시 객체를 반납 받아서 pool에 저장하는 프로그래밍 기법
- 전에 데이터베이스와 이미 연결이 수립된 다수의 커넥션들이 존재한다.
- 커넥션 풀 안의 커넥션들은 데이터베이스 요청이 들어올 때 마다 새롭게 연결을 수립하고 닫는대신 항상 연결을 열린 상태로 유지한다.

1. Container 구동 시 일정 수의 Connection 객체를 생성
2. 클라이언트의 요청에 의해 애플리케이션이 Connection Pool에서 Connection객체를 받아와 DBMS작업을 수행
3. 작업이 끝나면 Connection Pool에 Connection객체를 반납

#### HikariCP

HikariCP는 스프링부트에 기본으로 내장되어 있는 JDBC 데이터베이스 커넥션 풀링 프레임워크이다.
HikariCP는 바이트코드 수준까지 극단적으로 최적화 되어있다. 