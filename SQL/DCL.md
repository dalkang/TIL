# SQL

## DCL

* 데이터 사용 권한 관리
* 데이터베이스 트랜젝션 명시(완료/취소)
* 권한 부여 및 취소
  * * `GRANT` : 데이터베이스 객체에 권한 부여
    * `REVOKE` : 이미 부여된 데이터베이스 객체의 권한 취소 



## Role

### CONNECT ROLE

* 사용자가 데이터베이스에 접속 가능하도록 하기 위해서 가장 기본적인 `시스템 권한` 8가지를 묶어 놓은 롤
  * ALTER SESSION
  * CREATE TABLE
  * CREATE VIEW
  * Etc

### RESOURCE ROLE

* 사용자가 객체(테이블, 뷰, 인덱스 등)를 생성할 수 있도록 하기 위해서 `시스템 권한`을 묶어 놓은 것
  * CREATE TABLE
  * CREATE SEQUENCE
  * CREATE PROCEDURE
  * CREATE TRIGGER
  * CREATE CLUSTER



### DBA ROLE

* 시스템 관리에 필요한 모든 권한을 부여할 수 있는 강력한 권한의 롤
* 데이터베이스 객체 관리, 사용자 관리 등의 모든 권한을 보유
* 시스템 자원 무제한적으로 사용 가능



## Oracle

* 오라클 설치 후 기본적으로 DBA롤이 부여된 SYS, SYSTEM 계정 생성

### SYS 계정

* Oracle DB 관리자로 SUPER USER
* Oracle 시스템을 유지, 관리, 생성하기 위한 모든 권한을 갖는 계정
* DB 생성/삭제 가능
* SYSDBA 권한 가짐

### SYSTEM 계정

* 생성된 DB를 운영, 관리하기 위한 관리자 계정
* SYS와 유사한 권한을 가지고 있지만 DB 생성과 삭제 불가
* SYSOPER 권한 (운영)



## 권한

* 특정 유형의 sql문을 실행하거나 다른 사용자의 객체를 사용할 수 있는 권리

### 시스템 권한

> DBA 계정에 부여
>
> 사용자 생성/삭제, DB 접근 및 각종 객체 생성 권한

* `CREATE USER` : 계정 생성
* `DROP USER` : 계정 삭제
* `CREATE TABLE` : 테이블 생성
* `CREATE VIEW` : 뷰 생성
* `CREATE SEQUENCE` : 시퀀스 생성
* `CREATE PROCEDURE` : 프로시저 생성
* `DROP ANY TABLE` : 임의 테이블 삭제



### 사용자 권한

```sql
-- 사용자 계정 생성
CREATE USER newUser IDENTIFIED BY "1234"
	DEFAULT TABLESPACE USERS
	TEMPORARY TABLESPACE TEMP;
	-- 생성할 때 할당량 지정 가능
	-- QUOTA 50M ON USERS;
```

```sql
-- 사용자에게 ROLE(connect, resource) 부여
GRANT connect, resource TO newUser;
```

```sql
-- 사용자 계정 생성 후 TABLESPACE 할당량 지정
ALTER USER newUser QUOTA unlimited ON USERS;
-- 할당량이 무제한일 경우 MAX_BYTES가 -1
```

```sql
-- newUser의 할당량 50M로 변경
ALTER USER newUser QUOTA 50M ON USERS;
```

```sql
-- NEWUSER 할당량 확인 (NEWUSER의 WORKSHEET)
SELECT * FROM USER_TS_QUOTAS;
```

```sql
-- 사용자 계정 정보 변경
ALTER USER newUser IDENTIFIED BY 1111;
```

```sql
-- 현재 사용자 조회
SHOW USER;
```

```sql
-- DBA 권한으로 현재 생성된 계정 조회
-- DBA 권한 없는 사용자가 조회하면 오류 발생
SELECT * FROM DBS_USERS;
```

```sql
-- DBA 권한보다 낮은 모든 사용자 권한으로 사용자 목록 조회
SELECT * FROM ALL_USERS;
```

```sql
-- 현재 사용자 권한으로 조회
SELECT * FROM USER_USERS;
```

```sql
-- 사용자 계정 삭제 : 현재 접속하고 있는 사용자 계정은 삭제 불가, 오류 발생
DROP USER newUser;
```

```sql
-- 제약 조건까지 삭제
DROP USER newUser CASCADE;
```

```sql
-- 사용자 계정 잠금
ALTER USER SQLDB ACCOUNT LOCK;
```

```sql
-- 사용자 계정 잠금 해제
ALTER USER SQLDB ACCOUNT UNLOCK;
```



### 테이블 권한

```sql
-- DBA 권한으로 DB 전체의 모든 객체에 대한 정보 조회
SELECT * FROM DBA_TABLES;
```

```sql
-- 현재 사용자가 자신이 생성한 객체와 다른 사용자가 만든 객체 중에서
-- 자신에게 부여된 권한으로 볼 수 있는 정보 조회
SELECT * FROM ALL_TABLES;
```

```sql
-- 자신이 소유하고 있는 DB 확인(객체 정보 확인)
SELECT * FROM TAB;
```



### 객체 권한

> 특정 객체를 조작할 수 있는 권한

* **DML** 사용 

  * `SELECT` : 조회 권한

    ```sql
    -- ex) newUsr에게 HR 계정의 employees 테이블 조회할 수 있는 권한 부여
    GRANT SELECT ON HR.employess To newUser;
    ```

  * `INSERT` : 데이터 삽입 권한

  * `UPDATE` : 데이터 수정 권한

    ```sql
    -- ex) newUsr에게 HR 계정의 employees 테이블 수정할 수 있는 권한 부여
    GRANT UPDATE ON HR.employess To newUser;
    ```

  * `DELETE` : 데이터 삭제 권한

* 롤 부여 : `GRANT` 명령어 사용

  > GRANT 권한 유형 ON 스키마/테이블 TO 사용자;

* 롤 제거 : `REVOKE` 명령어 사용

  > REVOKE 권한 유형 ON 스키마/테이블 FROM 사용자;

  ```sql
  -- ex) newUsr에게 HR 계정의 employees 테이블 조회할 수 있는 권한 제거
  REVOKE SELECT ON HR.employees FROM newUser;
  ```

* 롤 조회

  ```sql
  SELECT grantee, granted_role FROM DBA_ROLE_PRIVS;
  ```