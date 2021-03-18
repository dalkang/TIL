## Multi thread

> 하나의 프로세스에서 두 가지 이상의 작업을 처리하는 것

* 하나의 프로세스 내부에 **다수의 스레드**가 생성되기 때문에 하나의 프로세스가 예외를 발생시키면 프로세스 자체가 종료될 수도 있음
  * 이를 방지하기 위해 **예외 처리**를 해줘야 함



## Main thread

> main() 메소드를 실행하며 시작

* 멀티 스레드를 생성해서 멀티 태스킹 수행
* 메인 스레드가 먼저 종료되어도 **수행중인 작업 스레드가 있다면 프로세스는 종료되지 않음**



## Thread class

>  **java.lang.Thread 클래스로부터 작업 스레드 객체 직접 생성하기**

* `Runnable`을 매개값으로 갖는 생성자 호출

  ```java
  Thread thread = new Thread(Runnable target);
  ```

  * **인터페이스 타입**이기 때문에 **구현 객체**를 만들어 대입

* `Runnable` 익명 구현 객체를 생성한 후, **run()을 재정의**해서 작업 스레드가 실행할 코드 작성

  ```java
  Thread thread = new Thread(new Runnable() {
    public void run() {
      // 작업 스레드가 실행할 코드
    }
  });
  ```

* `Runnable` 인터페이스는 run() 메소드 하나만 정의되어 있어 함수적 인터페이스

  ```java
  // 람다식 활용 가능
  Thread thread = new Thread(() -> {
    // 작업 스레드가 실행할 코드
  });
  
  // Java8부터 지원, 즉 이전 버전에서는 사용 불가
  ```

* start() 메소드로 호출

  ```java
  thread.start();
  ```

  * 호출되면 매개값으로 받은 `Runnable` 의 run() 메소드 실행

<hr/>

>  **Thread 하위 클래스로 작업 스레드 객체 생성하기**

* Thread 클래스 상속한 후 run 메소드를 overriding

  ```java
  public class WorkerThread extends Thread {
    @Override
    public void run() {
      // 작업 스레드가 실행할 코드
    }
  }
  
  Thread thread = new WorkerThread();
  
  thread.start();
  ```

* 익명 자식 객체 활용 가능

  ```java
  Thread thread = new Thread() {
    public void run() {
      // 작업 스레드가 실행할 코드
    }
  }
  ```

  

## Thread 이름

> 디버깅할 때 어떤 스레드가 어떤 작업을 하는지 확인하는 용도

* 메인 스레드: `main`

* 직접 생성한 스레드: `Thread-n`

  * `n` : 스레드의 번호

* 스레드 이름 변경

  ```java
  thread.setName("스레드 이름");
  ```

* 스레드 이름 가져오기

  ```java
  thread.getName();
  ```



## Thread 우선순위

> 멀티 스레드는 동시성(Concurrency)와 병렬성(Parallelism)으로 실행

* 동시성: 멀티 작업을 위해 하나의 코어에서 멀티 스레드가 번갈아가며 실행
* 병렬성: 멀티 작업을 위해 멀티 코어에서 개별 스레드 동시에 실행



### 스케쥴링

> 스레드의 개수가 코어의 수보다 많을 경우, 스레드를 어떤 순서에 의해 동시성으로 실행할지 결정하는 것

1. `우선순위(Priority) 방식` : 우선순위가 높은 스레드가 실행 상태를 더 많이 갖도록 함

   * 스레드 객체에 우선순위 번호 부여하기에 **코드로 제어 가능**

   * 우선순위를 부여하지 않을 경우 모든 스레드는 기본적으로 5 우선순위 할당

   * 변경하고 싶다면 Thread 클래스가 제공하는 setPriority() 메소드 이용

     ```java
     thread.setPriority(우선순위)
     ```

2. `순환 할당(Round-Robin) 방식`: 스레드를 정해진 시간만큼 실행 후 다른 스레드 실행

   * JVM에 의해 정해지기에 **코드로 제어 불가능**

