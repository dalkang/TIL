# Annotation

* Annotation을 사용하기 위해서는 context 네임스페이스 추가
  * application-context.xml 생성한 후 아래쪽에 Namespaces 탭 선택
    * context 체크
  * Bean 생성

## Bean 생성과 관련 Annotation

> Bean 생성(설정)을 위해 Class 위에 추가되는 Annotation
>
> Class 이름 위에 붙이면 해당 Class에 대한 Bean 자동 생성
>
> Bean의 이름은 Class 이름에서 첫 문자 소문자 (ex: NameService의 Class Bean 이름은 nameService)
>
> Annotation 이용하기 위해 xml 설정 파일에 context namespace 추가
>
> ```java
> <context:component-scan base-package="packageName" />
> ```

* `@Component`

  > Class를 Bean으로 등록
  >
  > Bean id 지정 가능
  >
  > ```java
  > @Component("빈이름") == <bean id="빈 이름">
  > ```

    * `@Controller`
        * Controller Class에 사용
    * `@Service`
        * Service Class에 사용
    * `@Repository`
      * DAO Class 또는 Repository Class에 사용

* `@Configuration`

  * Bean 설정 클래스임을 나타내는 어노테이션

* `@Bean`

  * Bean을 생성해서 반환해주는 어노테이션
  * Bean을 생성하는 메소드 앞에 기술
  * `@Bean`이 붙은 메소드는 반드시 Bean 반환

## DI 관련 Annotation

> xml 설정 파일에 있는 <bean>에 대해 DI 하거나, 자바 코드에서 생성된 Bean에 대해 DI할 수 있음

* `@Autowired`

  > Class 선언부에 위치할 경우 : 기본 생성자를 호출하며 주입
  >
  > Setter 메소드 위에 위치할 경우 : Setter 메소드 호출하며 주입

  * 타입을 기준으로 의존성 주입
  * Spring Bean에 의존하는 다른 Bean을 자동으로 주입할 때 사용
  * Spring이 지원

* `@Inject`

  * `@Autowired`와 동일
  * Java에서 지원

* `@Qualifier`

  * 특정 Bean의 이름 지정
  * 동일한 Interface를 구현한 클래스가 여러 개 있는 경우, 사용하고자 하는 특정 Bean의 이름을 지정할 때 사용

* `@Resource`

  * `@Autowired`와 `@Qualifier`를 사용하는 것과 동일
  * Java에서 지원