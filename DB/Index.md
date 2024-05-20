## Index

추가적인 쓰기 작업과 저장 공간을 활용하여  테이블에 저장된 데이터의 검색 속도를 향상시키기 위한 자료구조이다.

인덱스는 데이터베이스 내의 특정 컬럼(열)이나 컬럼들의 조합에 대한 값과 해당 값이 저장된 레코드(행)의 위치를 매핑하여 데이터베이스 쿼리의 성능을 최적화하는 역할을 한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd14zYz%2Fbtr1T50cGOH%2FFuiV4YvHGsIsHqpHYCTR6K%2Fimg.png)

#### 사용하는 이유

- WHERE 구문과 일치하는 열을 빨리 찾기 위해
- 특정 열을 고려 대상에서 빨리 없애버리기 위해
- 조인을 실행할 때 다른 테이블에서 열을 추출하기 위해
- 특정하게 인덱스 된 컬럼을 위한 MIN() 또는 MAX()값을 찾기 위해
- 데이터 열을 참조하지 않는 상태로 값을 추츨하기 위해서 쿼리를 촤적화하는 경우

#### Index 구조

- 논리적/물리적으로 테이블과 독립적
- 테이블은 컬럼에 데이터가 정렬되지 않고 입력된 순서대로 들어가지만, index는 KEY 컬럼과 ROWID 컬럼 두 개로 이루어져 있고 오름차순, 내림차순이 가능
> Key : 인덱스를 생성하라고 지정한 컬럼의 값
- FRM : 테이블 구조 저장 파일
- MYD : 실제 데이터 파일
- MYI : Index 정보 파일 (Index 사용 시 생성)

#### index 스캔 방식

1. Index Range Scan

- 인덱스 루트 블록에서 리프 블록까지 수직적으로 탐색한 후에 리프 블록을 필요한 범위만 스캔하는 방식
- 항상 빠른 속도 보장 X

![](https://mblogthumb-phinf.pstatic.net/MjAyMDA3MTBfMjAz/MDAxNTk0MzQ1NzMwOTk5.n-2L_Hdk0xPgUR2VwJgdxvjyr8baxlv_iu-FlaiFQuAg.qJZwRU35_LWjjXKLvhhOfUq3X4XxjMyWYYV4KS4aQaIg.PNG.dnjswls23/image.png?type=w800)

2. Index Full Scan

- 수직적 탐색 없이 인덱스 리프 블록을 처음부터 끝까지 수평적으로 탐색하는 방식으로, 대개는 데이터 검색을 위한 최적의 인덱스가 없을 때 차선으로 선택하는 방식

 ![](https://mblogthumb-phinf.pstatic.net/MjAyMDA3MTBfMjQ3/MDAxNTk0MzQ1NzU3NzEz.3hCbgxchHpUYCb0LPX_CreZNGqr_jo3MsTIiQOlH9nwg.qOQVv-XdY5eMdKJO7Pi4dnDEkRs5eZP-aKY4jvAMQqkg.PNG.dnjswls23/image.png?type=w800)

3. Index Unique Scan

- 수직적 탐색만으로 데이터를 찾는 스캔 방식으로 '=' 조건일 때만 동작

![](https://mblogthumb-phinf.pstatic.net/MjAyMDA3MTBfMjU1/MDAxNTk0MzQ1ODIyMTA5.r9ttj2BX3EboPD5_ipr8J_Ni9NLmgri0DUapPDLgSOUg.QiMt7abp2YgHcCSUohZLSDMmoih2mSrpiElc45ID5fcg.PNG.dnjswls23/image.png?type=w800)

4. Index Skip Scan

- 인덱스 선두 컬럼이 조건절에 빠졌어도 인덱스를 활용하는 스캔방식
- 인덱스 선두 컬럼의 Distinct Value 개수가 적고 후행 컬럼의 Distinct Value 개수가 많을 때 유용

![](https://mblogthumb-phinf.pstatic.net/MjAyMDA3MTBfMTAw/MDAxNTk0MzQ2MDQ1Mzgy.dz5IgqHPbJyv7onij1mu8ERobwU5osXaMQKVX1WijBgg.mHgRoan9tX6vftkrKec-bAmL6xJrs4f3deYMeDSmNlcg.PNG.dnjswls23/image.png?type=w800)

5. Index Fast Full Scan

- 인덱스 트리 구조를 무시하고 인덱스 세그먼트 전체를 Multiblock Read 방식으로 스캔하기 때문에 Index Full Sacn 방식보다 빠름
- 결과 집합이 인덱스 키 순서대로 정렬되지 않음

![](https://mblogthumb-phinf.pstatic.net/MjAyMDA3MTBfMjQz/MDAxNTk0MzQ2MTcxMzMw.1zyT6K-sAcPQZJtd1o5Aq2KeVcNr1NJajg165CzHCCcg.AEeN6qqulNg-DH307vvbqGcAnwpN2UsvL5evZOv3YUgg.PNG.dnjswls23/image.png?type=w800)

#### 장점

- 검색 대상 레코드의 범위를 줄여 검색 속도를 빠르게 할 수 있다.
- 중복 데이터를 방지하거나 특정 컬럼의 유일성을 보장할 수 있다.
- ORDER BY 절과 GROUP BY 절, WHERE 절 등이 사용되는 작업이 더욱 효율적으로 처리된다.

#### 단점

- 인덱스 생성에 따른 추가적인 저장 공간이 필요하다.
- CREATE(삽입), DELETE(삭제), UPDATE(수정) 작업 시에도 인덱스를 업데이트해야 하므로 성능 저하가 발생할 수 있다.
- 한 페이지를 동시에 수정할 수 있는 병행성이 줄어든다.
- 인덱스 생성 시간이 오래 걸릴 수 있다.