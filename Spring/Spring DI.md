# Spring DI

## Spring DI Injection

>  Spring의 의존성 주입 : 스프링의 핵심 기능

- Spring 프레임워크는 컨테이너 역할을 하기 때문에, Spring Container(IoC Container)라고도 함
- Spring은 필요한 Bean을 생성해서 Container에 넣어 관리, 필요한 곳에 주입해 줌
- Bean을 생성하기 위한 설정과 의존 객체를 주입시키는 설정 필요
- 스프링은 Pre-loading 방식 사용
  - ApplicationContex를 이용해서 컨테이너를 구동하면 
  - 컨테이너가 구동되는 시점에 스프링 설정 파일에 등록된 빈을 생성하고 컨테이너에 로드

### 1. XML Injection

* XML을 이용해 의존성을 주입할 때
  * 생성자 기반 : 생성자가 반드시 있어야 함
  * Setter 기반 : Setter 메소드가 반드시 있어야 함

1. XML 파일에 Bean을 정의 (생성)

   ```java
   <bean id="빈이름" class="패키지명.클래스명">
   ```

2. 의존성 설정 (부품 조립 : DI )

   ```java
   <ref bean="의존하는 빈">
   ```

### 2. Annotation Injection

- xml 설정 파일에서 `<bean>` 태그를 이용해서 설정했던 Bean 설정을, Annotation(메타데이터)를 이용해서 자바 코드에서 설정

  > xml 설정 파일에서 Bean을 설정하지 않고 Spring이 자바 소스 코드를 읽어 클래스에 `@Component` 어노테이션이 붙은 클래스 객체화 (bean 설정)

- A1 클래스의 객체를 A2클래스의 객체로 변경하려면

  * A1 클래스에서 @Component을 제거
  * A2 클래스에 @Component을 붙이면 됨
  * @Autowired 어노테이션을 사용하여 bean 자동 삽입

* xml 설정 파일에 context namespace 추가
  > 빈 설정을 위한 어노테이션을 사용하기 위해서는 설정 파일에 context namespace가 추가되어 있어야 함
  * `<context:component-scan>` 태그 이용해서 Bean으로 등록될 클래스 패키지 지정
  * 의미 : 자바 소스 코드에서 @Component로 등록된 클래스를 찾아서(scan) 클래스를 객체화(빈 설정)

