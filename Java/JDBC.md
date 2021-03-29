# Java

## JDBC(Java Database Connectivity)

>  다양한 종류의 관계형 데이터베이스에 접근할 때 사용되는 자바 표준 SQL 인터페이스
>
> 응용 프로그램과 DBMS에서 연결 역할
>
> SQL문을 DBMS에 전달하고 결과값을 응용 프로그램에게 전달
>
> (SQL문 <-> JDBC Interface <-> JDBC Driver <-> DBMS)

* 자바 프로그램이 DBMS에 접근하여 작업할 수 있게 해주는 API를 제공하는 클래스 모음
* 모든 DBMS에서 공통적으로 사용할 수 있는 인터페이스와 클래스로 구성



## JDBC 구성

* JDBC 인터페이스
* JDBC 드라이버
  * JDBC 인터페이스를 구현한 클래스 파일 모음(jar 파일)
  * 각 DBMS에 맞는 JDBC 드라이버 사용
  * 각 DBMS 벤더에서 제공되는 구현 클래스
    * 실제 구현 클래스는 각 DBMS 벤더가 구현했기 때문에 거의 모든 벤더가 JDBC 드라이버 제공



## JDBC-ODBC Bridge Driver

> JDK에 포함된 Driver, 설치 불필요

* JDBC가 ODBC Driver에 접근하는 통로를 제공
* JDBC API로 작성된 프로그램이 동작하는 운영체제 내에 **반드시 OCBC 드라이버 존재해야 함**
* 실제로는 ODBC의 API를 구현해 데이터베이스를 연동
* JDBC를 통해 호출된 명령이 다시 ODBC를 통해 나가야 하기 때문에 두 개의 브릿지를 거치게 되어 빠른 성능을 요구하는 애플리케이션의 경우에는 적당하지 않음 (**속도 문제**)



## Native-API / Partly Java Driver

> 프로그램에서 로드하기 위해서 드라이버 설치, CLASSPATH 지정 필요

* Native-API는 **벤더에서 제공하는 라이브러리를 이용해 DB에 액세스 한다**는 의미
* C, C++ 등으로 만들어진 Native Library를 호출해 DB 연결
* 인터페이스만 자바로 구현
* JDBC API 호출을 특정 데이터베이스의 클라이언트 호출 API로 바꿔주는 드라이버
* JDBC 프로그램이 동작하는 클라이언트 측에 C, C++ Native API 설치해야 함
* **시스템에 종속적**이고 **드라이버에 문제가 있을 경우 심각한 오류 발생 가능**
* 자체 라이브러리(Native Library)를 사용하기 때문에 JDBC-ODBC Bridge 드라이버보다는 빠름



## Net-protocol all-Java Driver

> 프로그램에서 로드하기 위해서 드라이버 설치, CLASSPATH 지정 필요
>
> 순수한 자바

* 클라이언트의 JDBC API 호출을 DBMS에 독립적인 프로토콜로 바꾸어 서버로 전송
* 서버에서 미들웨어가 해당 프로토콜을 특정 데이터베이스의 API로 바꾸어 처리
* 드라이버가 바로 DB를 제어하지 않고 정해진 프로토콜을 사용하는 **미들웨어**를 통하여 DB를 제어
* 서버의 미들웨어가 맡는 역할은 서버의 다양한 DB에 접근할 수 있도록 클라이언트의 요청을 처리하는 임무 (유연성 제공)
* 클라이언트에 소프트웨어 설치 불필요



## Native-Protocal / All-Java Driver

> 프로그램에서 로드하기 위해서 드라이버 설치, CLASSPATH 지정 필요
>
> 데이터베이스 프로토콜 드라이버

* JDBC API 호출을 미들웨어 거치지 않고 직접 DB Server의 프로토콜로 변환
* **DBMS의 고유한 프로토콜로 직접 변환**해 처리하기 때문에 처리 속도가 가장 빠름
* JDBC 드라이버와 DBMS간 1:1 관계
* **Oracle의 Thine Driver가 이 Type**
* 빠른 액세스를 요하는 2-tier 시스템 구성에 적합



## JDBC 효용성

* 사용하는 RDBMS로부터 독립적인 프로그래밍 가능
* 쉽게 RDBMS 교체 가능
* 자바에서 문자열로 query를 전달할 뿐, 해석은 각각의 벤더가 구현한 Driver에서 담당
* 표준 SQL 뿐만 아니라 각각의 JDBC Driver를 제공하는 DBMS 벤더별로 최적의 성능 발휘할 수 있는 벤더 종속적인 SQL에 대한 처리도 가능



## JDBC 연결 과정

> 1. 드라이버 로드
> 2. Connection 객체 생성
> 3. Statement 객체 생성
> 4. 쿼리 (Query) 수행
> 5. SQL문에 결과 반환이 있는 경우 ResultSet 객체로 결과 획득
> 6. 모든 객체 close()
>    * ResultSet
>    * Statement
>    * Connection (접속 종료)

### 1. 드라이버 로드

```java
Class.forName("oracle.jdbc.driver.OracleDriver");
```

### 2. Connection 객체 생성

> 오라클 서버 실제 연결
>
> Connection 객체가 생성되면 DBMS 접속 성공

* DriverManager 클래스의 static 메소드인 getConnection() 메소드를 이용해 Connection 객체 생성
* `url` : "jdbc:oracle:thin:@localhost:1521:xe"
  * **jdbc:oracle:thin:** - JDBC 드라이버
    * **jdbc** - JDBC URL의 프로토콜 이름
    * oracle:thin - 오라클 JDBC 드라이버
  * **@localhost** - 오라클이 설치된 IP (호스트 이름)
    * @localhost
    * 127.0.0.1
  * **1521** - 오라클 접속 포트
    * listener.ora 파일에서 확인 (NETWORK₩ADMIN)

```java
String url = "jdbc:oracle:thin:@localhost:1521:xe";
String user = "user";
String pwd = "1234";

Connection connection = DriverManager.getConnection(url, // 오라클 주소
                           			    user, // 계정명
                            			    password // 비밀번호);
```

### 3. Statement 객체 생성

> 쿼리문 전송을 위한 Statement 또는 PreparedStatement 객체 생성

* Connection 인터페이스의 createStatement() 메소드를 사용해 객체 생성
* Statement 객체를 통해 SQL 전송 가능

```java
Statement statement = connection.createStatement();
```

### 4. Query 수행 

> SQL문 실행 결과가 다르기 때문에 두 가지로 메소드 구분해서 사용

* `executeQuery() `
  * **SELECT** 구문에서 사용
  * **ResultSet** 객체로 반환 (SELECT문 결과에 해당되는 여러 행 반환)
* `executeUpdate()`
  * **INSERT** , **UPDATE** , **DELETE** 구문에서 사용
  * 영향을 받은 행의 수 반환

### 5. ResultSet 객체로부터 결과 획득

* **SELECT 구문의 경우**
  * `executeQuery()` 메소드 결과로 ResultSet 반환
  * ResultSet의 `next()` 메소드를 이용해 논리적 커서를 이동해가며 각 열의 데이터를 바인딩해 옴
  * `next()` 
    * 커서를 이동하면서 다음 행 지정
    * 시작 시 첫 번째 행 이전에 위치해 있다가 `next()` 메소드가 호출되면 이동하면서 첫 데이터를 기리킴
    * **리턴 타입은 boolean**
      * 다음 행이 있으면 true, 없으면 false 반환
      * 모든 행을 가져오려면 반복문 사용
  * `getXXX()`
    * 실제 값을 가져오는 메소드
    * 데이터 타입에 맞춰 사용
      * getString()
      * getInt()
      * getDate()

### 6. 모든 객체 close()



## Statement Interface

>  executeQuery() / executeUpdate()

* 문자열로 구성된 sql문을 dbms로 전송하면, JDBC 드라이버가 내부적으로 sql을 읽을 수 있는 형식으로 전처리 수행(precompile)

* sql은 실행될 때마다 매번 전처리 수행하기 때문에 반복적인 작업에서 속도 늦어짐

* 쿼리문에 값이 다 입력되어 있어야 함 >> 쿼리문 작성 시 복잡



## PreparedStatement Interface

> Statement를 상속받은 하위 인터페이스

* SQL문이 실행될 때마다 매번 전처리하지 않고 라이브러리 캐시에서 읽어와서 처리하기 때문에 처리 속도가 Statement보다 좋음
* sql문과 구조는 같지만 주건이 수시로 변할 때 조건의 변수 처리를 '?' (placeholder)로 지정 >> 바인딩 변수
* 동일한 SQL문을 특정 값만 바꾸어서 여러 번 실행할 때, 인수가 많아서 SQL문이 복잡할 때 PreparedStatement를 사용하는 게 편리
* 바인딩 변수의 순서는 '?'의 순서에 의해 결정
* 시작번호는 1부터 시작
* 바인딩 변수에 값을 저장하기 위해 `setXXX()` 메소드 사용
* 바인딩 변수는 열 이름이 아닌 열의 값 위치에 적음



<hr/>



## DB 연결 전용 클래스 생성

* 싱글톤 패턴 이용 : 한 개의 객체만을 생성

  > **DB 연결 과정이 부하가 가장 심하기 때문**
  >
  > 한 번 생성된 연결 객체 계속 사용하기 위해

* Connection 객체 생성해서 반환하는 메소드 사용

  * getConnection() : 연결한 객체 반환

* 자원 반환 메소드 close()

  > 자원 수에 따라 close() 메소드 오버로딩 방식으로 사용

```java
package JDBC.sec02;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

// DB 연결 전용 클래스
public class DBConnect {
	// SingleTone : 객체 한 개만 생성 (private)
	private static Connection connection;
	
	// Connection 객체 생성 후 반환하는 메소드
	public static Connection getConnection() {
		// 연결 정보 문자열 생성
		String url = "jdbc:oracle:thin:@localhost:1521:xe";
		String user = "YOUNG";
		String pwd = "1234";
		// Connection 객체 생성
		// JDBC 4.0부터는 자동 로드
		try {
			connection = DriverManager.getConnection(url, user, pwd);
		} catch (SQLException e) {
			System.out.println("DB 연결 실패");
			e.printStackTrace();
		}
		return connection;
	}
	
	// 자원 반환 메소드
	// 반환되는 자원에 따라 다르게 메소드 오버로딩
	// Connection, PreparedStatement, ResultSet 자원 3개 반환 close() 메소드
	public static void close(Connection connection, PreparedStatement prepared, ResultSet resultSet) {
		try {
			if (resultSet != null) {				
				resultSet.close();
			}
			if (prepared != null) {
				prepared.close();
			}
			if (connection != null) {
				connection.close();
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	// 반환되는 자원에 따라 다르게 메소드 오버로딩
  // Connection, PreparedStatement 자원 2개 반환 close() 메소드
	public static void close(Connection connection, PreparedStatement prepared) {
		try {
			if (prepared != null) {
				prepared.close();
			}
			if (connection != null) {
				connection.close();
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}	
}
```



## SELECT

```java
package JDBC.sec02;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class BookSelectPreEx {

	public static void main(String[] args) {
		Connection connection = null;
		PreparedStatement pstmt = null;
		ResultSet resultSet = null;
		
		try {
			// 객체 연결
			connection = DBConnect.getConnection();
			
			System.out.println("도서 정보 조회");
			
			// sql문 작성
			String sql = "SELECT * FROM book ORDER BY bookNo";
			pstmt = connection.prepareStatement(sql);
			resultSet = pstmt.executeQuery();
			
			// resultSet에게 다음 행이 있으면 true, 없으면 false
			while (resultSet.next()) {
				String bookNo = resultSet.getNString(1);
				String bookName = resultSet.getNString(2);
				String bookAuthor = resultSet.getNString(3);
				int bookPrice = resultSet.getInt(4);
				String bookDate = resultSet.getNString(5);
				int bookStock = resultSet.getInt(6);
				String pubNo = resultSet.getNString(7);
				
				// 한 행씩 출력
				System.out.format("%-10s\t %-25s\t %-10s\t %6d\t %13s\t %3d\t %10s", 
						bookNo, bookName, bookAuthor, bookPrice, bookDate, bookStock, pubNo);
			
				System.out.println();
				
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			DBConnect.close(connection, pstmt, resultSet);
		}
	}
}
```



## INSERT

```java
package JDBC.sec02;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.util.Scanner;

public class BookInsertPreEx {

	public static void main(String[] args) {
		// DBConnect 클래스의 메소드로 객체 생성, DB 연결 확인
		Connection connection = DBConnect.getConnection();
		// 전체 스코프에서 사용하기 위해 최상단에 선언, 생성
		PreparedStatement pstmt = null;
		
		if (connection!= null) {
			System.out.println("DB 연결 성공");
		}
		
		// 도서 정보 입력 받아 인스턴스로 저장
		Scanner sc = new Scanner(System.in);
		
		System.out.println("**********도서 정보 등록**********");
		
		System.out.print("도서 번호 : ");
		String bookNo = sc.nextLine();
		
		System.out.print("도서명 : ");
		String bookName = sc.nextLine();
		
		System.out.print("저자 : ");
		String bookAuthor = sc.nextLine();
		
		System.out.print("가격 : ");
		int bookPrice = sc.nextInt();
		
		sc.nextLine();
		
		System.out.print("출판일 (ex: YYYY/MM/DD) : ");
		String bookDate = sc.nextLine();
		
		System.out.print("재고 : ");
		int bookStock = sc.nextInt();
		
		sc.nextLine();
		
		System.out.print("출판사 : ");
		String pubNo = sc.nextLine();
		
		sc.close();
		
		// INSERT 쿼리문 수행
		try {
			// PreparedStatement 사용 : 열의 값 자리에 '?' 지정 > 바인딩 변수
			String sql = "INSERT INTO book VALUES(?, ?, ?, ?, ?, ?, ?)";
			
			// 쿼리문 전송을 위한 PreparedStatement 객체 생성
			pstmt = connection.prepareStatement(sql);
			
			// 바인딩 변수의 순서는 ?의 순서와 일치해야 함
			// 데이터 타입에 맞춰 저장되어야 하기 때문
			// 시작번호는 1부터
			// 바인딩 변수에 값을 저장하기 위해서는 setXXX() 메소드 사용
			pstmt.setString(1, bookNo);
			pstmt.setString(2, bookName);
			pstmt.setString(3, bookAuthor);
			pstmt.setInt(4, bookPrice);
			pstmt.setString(5, bookDate);
			pstmt.setInt(6, bookStock);
			pstmt.setString(7, pubNo);
			
			// insert, update, delete >> executeUpdate();
			pstmt.executeUpdate();
			
			System.out.println("도서 정보 등록 완료");
			
		} catch (Exception e) {
			System.out.println("도서 정보 등록 실패");
		} finally {
			DBConnect.close(connection, pstmt);
		}
	}
}
```



## UPDATE

```java
package JDBC.sec02;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.util.Scanner;

public class BookUpdatePreEx {

	public static void main(String[] args) {		
		// 수정할 도서 정보 입력
		Connection connection = null;
		PreparedStatement pstmt = null;
		
		// 도서 정보 입력 받아 인스턴스로 저장
		Scanner sc = new Scanner(System.in);
		
		System.out.println("**********도서 정보 수정**********");
		
		System.out.print("수정할 도서 번호 : ");
		String bookNo = sc.nextLine();
		
		System.out.print("도서명 : ");
		String bookName = sc.nextLine();
		
		System.out.print("저자 : ");
		String bookAuthor = sc.nextLine();
		
		System.out.print("가격 : ");
		int bookPrice = sc.nextInt();
		
		sc.nextLine();
		
		System.out.print("출판일 (ex: YYYY/MM/DD) : ");
		String bookDate = sc.nextLine();
		
		System.out.print("재고 : ");
		int bookStock = sc.nextInt();
		
		sc.nextLine();
		
		System.out.print("출판사 : ");
		String pubNo = sc.nextLine();
		
		sc.close();
		
		// sql문 작성 : update문
		try {
			connection = DBConnect.getConnection();
			
			String sql = "UPDATE book SET bookName=?, bookAuthor=?,"
					   + "bookPrice=?, bookDate=?, bookStock=?, pubNo=?"
					   + "WHERE bookNo=?";
			pstmt = connection.prepareStatement(sql);
			
			// 열의 값 자리에 ?로 처리하고 setXXX() 메소드 사용해서 순서대로 값 지정
			pstmt.setString(1, bookName);
			pstmt.setString(2, bookAuthor);
			pstmt.setInt(3, bookPrice);
			pstmt.setString(4, bookDate);
			pstmt.setInt(5, bookStock);
			pstmt.setString(6, pubNo);
			pstmt.setString(7, bookNo);
			
			pstmt.executeUpdate();
			
			System.out.println("도서 정보 수정 완료");
			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			DBConnect.close(connection, pstmt);
		}
	}
}
```



## DELETE

```java
package JDBC.sec02;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.Scanner;

public class BookDeletePreEx {

	public static void main(String[] args) {
		// DB 연결
		Connection connection = null;
		PreparedStatement pstmt = null;
		
		// 삭제할 도서 번호 입력
		Scanner sc = new Scanner(System.in);
		
		System.out.println("**********도서 정보 삭제**********");
		
		System.out.print("삭제할 도서 번호 : ");
		String bookNo = sc.nextLine();
		
		
		sc.close();
		
		// sql문 작성
		try {
			connection = DBConnect.getConnection();
			String sql = "DELETE book WHERE bookNo = ?";
			pstmt = connection.prepareStatement(sql);
			pstmt.setString(1, bookNo);
			pstmt.executeUpdate();
			System.out.println("도서 정보 삭제 완료");
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			DBConnect.close(connection, pstmt);
		}
	}
}
```

