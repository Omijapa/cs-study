# Stored procedure

Status: Done

# 개념

<aside>
📜

**Procedure**

DB에서 자주 사용하는 SQL문들을 묶어서 하나의 이름으로 저장해두는 실행 단위 (함수 같은 개념)

**Stored Procedure**

DB 내부에 저장된 procedure

</aside>

---

# 예시

```sql
/*
	Procedure 생성
*/

CREATE PROCEDURE GetUserById
    @UserId INT
AS
BEGIN
    SELECT * FROM Users WHERE Id = @UserId;
END;

/*
	Procedure 실행 (MySQL 기준)
*/

CALL GetUserById(10);
```

---

# 장,단점

- 장점
    - 재사용성
        - 자주 쓰는 쿼리를 함수처럼 재사용 가능
    - 보안성
        - 사용자에게 테이블 접근 권한 대신 procedure 실행 권한만 주면 됨
    - 성능 향상
        - 미리 컴파일되어 실행 속도 빠름
    - 유지보수성
        - 비즈니스 로직이 DB안에 집중되어 관리가 용이함
- 단점
    - DB 확장 어려움
    - 데이터 분석 어려움

---

# Function vs Procedure

|  | Function | Procedure |
| --- | --- | --- |
| 반환값 | 없음(또는 여러 개 OUT 가능) | 반드시 1개 반환 |
| 호출 방식 | `EXEC`, `CALL` | 쿼리 내에서 사용 가능 (`SELECT 함수명()`) |
| 용도 | 데이터 처리(삽입, 수정, 삭제) | 계산 결과 반환 |
| DB 영향 | DML(INSERT, UPDATE 등) 가능 | 주로 읽기 전용 |

---

# 기타

### Springboot JPA로 Stored Procedure를 다루는 내용은 아래 링크를 참조하자~

https://velog.io/@hyeokkr/Spring-Boot-JPA%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%B4-Stored-Procedure-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0