# SQL Injection

Status: Done

# 개념

<aside>
📜

**SQL Injection**

외부에서 데이터베이스를 공격할 때 사용하는 치명적인 웹 보안 취약점

웹 브라우저의 로그인 창이나 검색창에 일반적인 텍스트가 아닌, 악의적인 SQL 구문을 섞어 입력하는 공격 방식이다.

서버가 이 입력값을 필터링 하지 않고 그대로 DB에 쿼리를 날리면, 해커의 의도대로 DB가 조작되거나 정보가 유출될 수 있다.

SQL Injection 종류와 방어에 대해 알아보자!

</aside>

---

# SQL Injection 종류

## Error based SQL Injection

공격자가 DB 시스템의 오류 메시지를 통해 시스템의 내부 정보를 획득하는 방법으로, 테이블 구조, 열 이름, DB 버전 등의 정보를 얻을 수 있다.

```sql
SELECT * FROM users WHERE username = '' OR 1=1; --' AND password = '';
```

- 일단 로그인 우회로 시스템에 접속한다.
    - ' 를 통해 where 절을 닫아준다.
    - '1=1'은 항상 참이므로 구문을 항상 참으로 만든다.
    - -를 이용해 그 뒤의 모든 쿼리문을 주석처리해준다.

```sql
' AND (SELECT 1 FROM (SELECT COUNT(*), CONCAT((SELECT database()), 0x3a, FLOOR(RAND(0)*2)) AS x FROM information_schema.tables GROUP BY x) AS y); --

```

- SQL 문이 작동하는 것을 확인하면 특정 정보를 얻기 위해, 데이터베이스에서 의도적으로 오류를 유발하는 SQL 코드를 삽입한다.
    - ' 를 통해 where 절을 닫아준다.
    - select~~ : 쿼리문에서 중복된 항목을 생성하려 시도하므로, MySQL에서 오류를 발생시킬 수 있다. 오류 메시지는 일반적으로 데이터베이스 이름과 같은 내부 정보를 포함한다.시스템 정보를 모두 불러오는 쿼리문으로 시도해도 된다.
    - -를 이용해 그 뒤의 모든 쿼리문을 주석처리한다.

## Union based SQL Injection

Union 연산자를 악용하여 공격자가 추가 데이터를 반환하게 하는 기법으로, 두 개 이상의 select 쿼리의 결과를 하나의 집합으로 결합하는 Union 명령어의 특성을 이용한다.

```sql
SELECT name, description FROM products WHERE category = '$category';
```

- 일반적인 검색 쿼리 예시

```sql
SELECT name, description FROM products WHERE category = '' 
UNION SELECT username, password FROM users; --
```

- Union을 악용한 SQL Injection 예시
    - ' 를 통해 where 절을 닫아준다.
    - UNION SELECT~~ : 민감한 정보(사용자 이름, 비밀번호 해시)를 포함한 테이블을 찾은 후, 이를 반환하도록 시도한다.
    - -를 이용해 그 뒤의 모든 쿼리문을 주석처리한다.

## Blind SQL Injection

응답 페이지에 DB의 결과가 바로 표시되지 않을 때 사용하는 공격 기법으로, Limit, SUBSTR, ASCII를 사용해서 조건이 참이면 페이지가 정상 출력되고 그렇지 않으면 출력되지 않음으로 구분할 수 있다. 

아래 일반적이지 않은 두 쿼리문에서 참/거짓 조건에 따라 응답시간이 달라지는 것을 이용하여 데이터를 추출할 수 있다.

```sql
SELECT * FROM users WHERE id = $id;
```

- 일반적인 조회 예시

```sql
SELECT * FROM users WHERE id = '1' AND (SELECT LENGTH(database())) = 8; -- ';
```

- (SELECT LENGTH(database())) = 8는 현재 데이터베이스의 이름 길이가 8인지 여부를 확인한다.- 이후는 주석 처리되어 무시된다.
- 데이터베이스 이름의 길이가 8일 때, id가 '1'인 사용자 정보를 반환한다.

```sql
SELECT * FROM users WHERE id = '1' AND (SELECT SUBSTRING(table_name, 1, 1) FROM information_schema.tables WHERE table_schema = DATABASE() LIMIT 0,1) = 'a'; -- ';
```

- SUBSTRING(table_name, 1, 1)는 table_name에서 첫 번째 문자를 추출한다.
- FROM information_schema.tables WHERE table_schema = DATABASE()은 현재 데이터베이스의 테이블 이름을 가져온다.
- LIMIT 0,1은 첫 번째 테이블만 조회한다.
- 만약 첫 번째 테이블 이름의 첫 번째 문자가 'a'라면, id가 '1'인 사용자 정보를 반환한다.

---

# SQL Injection 방어 기법

## Prepared Statement

쿼리의 문법적인 틀을 먼저 만들어 컴파일 해놓고, 사용자의 입력값은 나중에 데이터로서만 끼워 넣는 방식

1. 데이터베이스는 쿼리문과 데이터를 분리해서 처리한다. 
    1. 먼저 `SELECT * FROM users WHERE id = ? AND password = ?` 처럼 쿼리 구조를 미리 컴파일해 둡니다.
2. 이후에 해커가 `?` 자리에 `admin' OR '1'='1` 같은 악의적인 문자열을 넣더라도, DB는 이를 실행 가능한 SQL 명령어가 아니라 단순한 문자열 데이터로 취급한다. 
    1. 즉, 아이디가 진짜로 `admin' OR '1'='1`인 사용자를 찾으려 시도할 뿐, 쿼리 구조가 무너지지 않는다.

## ORM (Object-Relational Mapping)

스프링 부트의 JPA(Hibernate)나 Node.js의 Sequelize, Prisma 같은 ORM 기술을 사용하면 애플리케이션 레벨에서의 SQL Injection을 방어할 수 있다.

- 개발자가 SQL 쿼리를 직접 문자열로 짜맞추는(String Concat) 대신, 객체 지향적인 코드로 데이터를 다루면 ORM 내부에서 자동으로 쿼리를 생성해 준다.
- 대부분의 ORM은 내부적으로 미리 Prepared Statement를 사용하도록 설계되어 있기 때문에, 기본 설정만으로도 SQL 인젝션을 강력하게 방어할 수 있다.

## 입력값 검증 및 필터링 (Sanitization)

사용자가 입력한 값이 우리가 기대한 형식과 일치하는지 서버에 도달하기 전이나 컨트롤러 단에서 미리 검사한다.

- 화이트리스트(White-list) 검증: 허용된 문자만 입력받도록 강제한다. 이메일 정규식, 숫자만 허용 등 엄격한 기준을 적용하면 악의적인 특수문자 삽입을 원천 차단할 수 있다.
- 특수문자 이스케이프(Escape): 쿼리 구조를 깨뜨릴 수 있는 기호들(작은따옴표 `'`, 큰따옴표 `"`, 세미콜론 `;`, 주석 `-` 등)을 일반 문자로 인식되도록 변환(예: `\'`)합니다.

## 그 외

- 에러 메시지 은닉
- DB 권한 최소화