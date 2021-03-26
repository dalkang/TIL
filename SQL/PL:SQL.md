# SQL

## PL/SQL

> Oracle's Procedual Language Extension to SQL의 줄임말

* **오라클의 내장된 절차적 언어**
* **프로시저로 만들어서 호출해서 사용**
* 데이터 베이스 응용 프로그램을 작성하는 데에 사용
* SQL 문에 변수, 제어, 입출력 등의 **프로그래밍 기능을 추가**하여 SQL 만으로 처리하기 어려운 문제 해결
* **컴파일 한 후 결과 실행**

### 구문 형식

```sql
-- 화면 출력 허용 설정 (접속 연결 동안 1회 수행 : 첫 접속 때 수행)
SET SERVEROUTP ON;

DECLARE
	변수명1 데이터타입 -- 선언
BEGIN
	실행 코드 -- 실행
END;
```

#### 테이블의 열의 값을 변수에 대입

```sql
DECLARE
    -- book 테이블의 bookPrice 타입으로 선언
    bPrice book.bookPrice%TYPE;
BEGIN
    -- bookPrice 열의 값을 알아와서 변수 bPrice에 저장, 출력
    SELECT bookPrice INTO bPrice 
    FROM book 
    WHERE bookName = '자바 프로그래밍';
    -- 변수값 출력
    DBMS_OUTPUT.PUT_LINE(bPrice);
END;
```

#### 상수

> CONSTANT 키워드 사용
>
> 상수는 프로그램 실행 중에 값 변경 불가

```sql
DECLARE
    -- 변수 선언 및 초기화
    price NUMBER(10) := 100;
    point NUMBER(5);
    
    --상수 rate 초기화
    rate CONSTANT NUMBER(2, 2) := 0.03;
BEGIN
    price := 200; -- 변수값은 변경 가능, rate는 상수이므로 변경 불가능
    point := price * rate;
    DBMS_OUTPUT.PUT_LINE('적립포인트:' || point);
END;
```

#### IF문

```sql
DECLARE
    bStock NUMBER(10);
BEGIN
    -- book 테이블의 bookStock 열의 값을 변수 bStock에 저장
    -- 조건 : bookNo가 '1003'인 도서의 bStock
    SELECT bookStock INTO bStock 
    FROM book 
    WHERE bookNo = '1003';
    
    IF bStock >= 7 THEN
        DBMS_OUTPUT.PUT_LINE('재고 수준 위험');
    ELSE
        DBMS_OUTPUT.PUT_LINE('재고 수준 보통');
    END IF;
END;
```

#### CASE문

> 조건에 따라 결과값 반환, 조건이 여러 개인 경우 사용

* SELECT절에서 반환값을 열로 사용

```sql
SELECT C.clientName, SUM(B.bookPrice * bsQty) AS "총 주문 금액",
    -- 이 다음 열 값으로 CASE문의 반환값으로 씀
    CASE
        WHEN SUM(B.bookPrice * bsQty) >= 200000 THEN '최우수'
        WHEN SUM(B.bookPrice * bsQty) >= 100000 THEN '우수'
        WHEN SUM(B.bookPrice * bsQty) >= 50000 THEN '일반'
        ELSE '관심고객'
    END AS "고객등급"
    
    FROM bookSale BS
        INNER JOIN client C ON C.clientNo = BS.clientNo
        INNER JOIN book B ON B.bookNo = BS.bookNo
    GROUP BY C.clientNo, C.clientName
    ORDER BY "총 주문 금액" DESC;
```

* 반환값을 변수에 저장해서 사용

```sql
DECLARE
    bStock NUMBER(5);
    resultValue VARCHAR2(10);
BEGIN
    -- book 테이블의 bookStock열의 값을 변수 bStock에 저장
    SELECT bookStock INTO bStock FROM book WHERE bookNo = '1003';
    -- 재고 출력
    DBMS_OUTPUT.PUT_LINE('재고:'||bStock);
    
    resultValue := CASE
                       WHEN bStock >= 7 THEN '위험'
                       WHEN bStock >= 4 THEN '보통'
                       ELSE '재고 수준 우수'
                   END;
                   
    -- resultValue 출력
    DBMS_OUTPUT.PUT_LINE('재고 수준'||resultvalue);
END;
```

#### LOOP문

```sql
DECLARE
    i NUMBER(5) := 0;
BEGIN
    LOOP
        -- 증감값
        i := i + 1;
        -- 반복 수행되는 문장
        DBMS_OUTPUT.PUT_LINE(i);
        -- 종료 조건
        EXIT WHEN i >= 10;
    END LOOP;
END;
```

#### FOR ~ LOOP문

```sql
DECLARE
    i NUMBER(5);
    result NUMBER(5) := 0;
BEGIN
    FOR i IN 1 .. 100
    LOOP
        result := result + i;
    END LOOP;
        DBMS_OUTPUT.PUT_LINE(result);
END;
```



## 예외처리

> 1. 사전에 정의된 예외 : 예외 종류가 미리 정의되어 있음, 자동 발생
> 2. 사용자 정의 예외 : 사용자가 발생 가능한 에외명(예외 종류)를 블록에 선언하고 에외 발생 시키는 것

### 예외 처리 구문

```sql
EXCEPTION
	WHEN 예외 종류 THEN
		예외 발생 시 수행 문장
```

#### 사용자 정의 예외

1. 예외 이름 선언 (선언부)
2. RAISE문을 사용해서 예외 발생
3. 예외 처리 구문에서 예외 처리

```sql
DECLARE
    -- 사용자 정의 예외형 선언
    BOOKNO_NOT_FOUND EXCEPTION;
BEGIN
    -- 존재하지 않는 도서번호로 도서 삭제 할 경우 예외 처리
    DELETE FROM book WHERE bookNo = '101010';
    
    -- SQL%NOTFOUND : 해당 SQL문에 영향을 받는 행의 수가 없을 경우 TRUE 반환
    IF (SQL%NOTFOUND) THEN
        RAISE BOOKNO_NOT_FOUND;
    ELSE
        DBMS_OUTPUT.PUT_LINE('삭제 성공');
    END IF;
    
    -- 도서 번호가 존재하지 않는 경우 예외 발생 > 예외 처리
    EXCEPTION 
        WHEN BOOKNO_NOT_FOUND THEN
            DBMS_OUTPUT.PUT_LINE('없는 도서번호입니다.');
END;
```

#### 사전에 정의된 예외

* `TOO_MANY_ROWS` : 결과가 하나보다 많은 행 반환할 때 발생
* `NO_DAYA_FOUND` : 결과가 0을 반환할 때 발생
* `DUP_VAL_ON_INDEX` : 고유 인덱스를 갖는 열에 중복값을 입력했을 때 발생
* `VALUE_ERROR` : 산술, 변환, 절삭, 크기 제한 오류가 발생했을 때 발생

```sql
DECLARE
    bAuthor book.bookAuthor%TYPE;
BEGIN
    SELECT bookAuthor INTO bAuthor 
    FROM book
    WHERE bookAuthor LIKE ('홍%');
    DBMS_OUTPUT.PUT_LINE('저자:'||bAuthor);
    
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            DBMS_OUTPUT.PUT_LINE('해당 저자가 없습니다.');
        WHEN TOO_MANY_ROWS THEN
            DBMS_OUTPUT.PUT_LINE('해당 저자가 2명 이상입니다.');
END;
```

