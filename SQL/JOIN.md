# SQL

## JOIN

* 여러 개의 테이블을 결합하여 조건에 맞는 행 검색

### INNER JOIN

> 주로 활용되는 JOIN문

* 공통되는 열이 있을 때 사용

* 공통 속성의 속성 값이 동일한 튜플만 반환

  > Ex) 전체 고객 중에서 상품을 주문한 적이 있는 고객만 조회

### OUTER JOIN

* 공통되는 열이 없을 때 사용

* 공통된 속성을 매개로 하는 정보가 없더라도 버리지 않고 연산의 결과를 릴레이션에 표시

  > Ex) 주문한 적 없는 고객도 함께 조회

* 값이 없는 대응 속성에는 `NULL` 값 채워서 반환

* 좌측 외부 조인

  * 좌측 릴레이션 정보 유지

* 우측 외부 조인

  * 우측 릴레이션 정보 유지

* 완전 외부 조인

  * 두 릴레이션의 모든 정보 유지

### 조인 기본 형식

> 서버에게 찾고자 하는 열의 정확한 위치를 알려주어 성능 향상

```sql
SELECT 열 리스트
FROM 테이블명1
		 JOIN 테이블명2
		 ON 조인조건(기본키=외래키);
```

```sql
SELECT client.clientNo, client.clientName
FROM client
JOIN bookSale
ON client.clientNo = bookSale.clientNo
```



### 테이블 활용 코드 작성

#### CLIENT TABLE

| COLUMN_NAME   | DAYA_TYPE         | NULLABLE | DATA_DEFAULT | COLUMN_ID | COMMENTS |
| ------------- | ----------------- | -------- | ------------ | --------- | -------- |
| CLIENTNO      | NUMBER(38, 0)     | YES      | (null)       | 1         | (null)   |
| CLIENTNAME    | VARCHAR2(26 BYTE) | YES      | (null)       | 2         | (null)   |
| CLIENTPHONE   | VARCHAR2(26 BYTE) | YES      | (null)       | 3         | (null)   |
| CLIENTADDRESS | VARCHAR2(26 BYTE) | YES      | (null)       | 4         | (null)   |
| CLIENTBIRYTH  | DATE              | YES      | (null)       | 5         | (null)   |
| CLIENTHOBBY   | VARCHAR2(50 BYTE) | YES      | (null)       | 6         | (null)   |
| CLIENTGENDER  | VARCHAR2(26 BYTE) | YES      | (null)       | 7         | (null)   |

#### BOOKSALE TABLE

| COLUMN_NAME | DAYA_TYPE     | NULLABLE | DATA_DEFAULT | COLUMN_ID | COMMENTS |
| ----------- | ------------- | -------- | ------------ | --------- | -------- |
| BSNO        | NUMBER(38, 0) | YES      | (null)       | 1         | (null)   |
| BSDATE      | DATE          | YES      | (null)       | 2         | (null)   |
| BSQTY       | NUMBER(38, 0) | YES      | (null)       | 3         | (null)   |
| CLIENTNO    | NUMBER(38, 0) | YES      | (null)       | 4         | (null)   |
| BOOKNO      | NUMBER(38, 0) | YES      | (null)       | 5         | (null)   |

#### BOOK TABLE

| COLUMN_NAME | DAYA_TYPE         | NULLABLE | DATA_DEFAULT | COLUMN_ID | COMMENTS |
| ----------- | ----------------- | -------- | ------------ | --------- | -------- |
| BOOKNO      | NUMBER(38, 0)     | YES      | (null)       | 1         | (null)   |
| BOOKNAME    | VARCHAR2(50 BYTE) | YES      | (null)       | 2         | (null)   |
| BOOKAUTHOR  | VARCHAR2(26 BYTE) | YES      | (null)       | 3         | (null)   |
| BOOKPRICE   | NUMBER(38, 0)     | YES      | (null)       | 4         | (null)   |
| BOOKDATE    | DATE              | YES      | (null)       | 5         | (null)   |
| BOOKSTOCK   | NUMBER(38, 0)     | YES      | (null)       | 6         | (null)   |
| PUBNONO     | NUMBER(38, 0)     | YES      | (null)       | 7         | (null)   |



#### INNER JOIN 활용 코드

* 한 번이라도 도서를 주문한 적 있는 고객 조회

  ```sql
  SELECT * 
  FROM client
      -- bookSale 테이블과 조인
      INNER JOIN bookSale
      -- bookSale 테이블과 공통된 속성 선택, 조회
      ON client.clientNo = bookSale.clientNo;
  ```

* 한 번이라도 도서를 주문한 적이 있는 고객 번호와 고객명 조회

  > 중복되는 행은 한 번만 출력, 고객번호를 기준으로 오름차순 정렬

  ```sql
  SELECT UNIQUE C.clientNo, C.clientName
  FROM client C
      INNER JOIN bookSale BS
      ON C.clientNo = BS.clientNo
  ORDER BY C.clientNo;
  ```

* 주문된 도서에 대해 도서를 주문한 고객명과 도서명 출력

  ```sql
  FROM bookSale BS
      INNER JOIN client C ON C.clientNo = BS.clientNo
      INNER JOIN book2 B ON B.bookNo = BS.bookNo;
  ```

  ```sql
  SELECT C.clientName, B.bookName
  FROM client C, book B, bookSale BS
  WHERE C.clientNo = BS.clientNo AND B.bookNo = BS.bookNo;
  ```

* 도서를 주문한 고객의 고객 정보, 주문 정보, 도서 정보 출력

  > 주문번호 오름차순 정리

  ```sql
  SELECT BS.bsNo, BS.bsDate, C.clientNo, C.clientName, B.bookName, BS.bsQty
  FROM bookSale BS
      INNER JOIN client C ON C.clientNo = BS.clientNo
      INNER JOIN book2 B ON B.bookNo = BS.bookNo
  ORDER BY BS.bsNo;
  ```

  