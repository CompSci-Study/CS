# CS스터디 8회차 - Spring Boot 뜯어보기

날짜: 2025년 2월 27일

# 장점

## **1. 간편한 설정과 빠른 개발⭐**

- **기본 설정 제공**: `spring-boot-starter`를 사용하여 복잡한 XML 설정 없이 애플리케이션을 빠르게 설정 가능
- **자동 설정(Auto Configuration)**: Spring Boot가 환경을 자동 감지하여 필요한 설정을 적용
- **내장 WAS 제공**: Tomcat, Jetty, Undertow 등의 내장 웹 서버 포함 (별도 WAS 설치 불필요)
    
    ![image](https://github.com/user-attachments/assets/f603318d-6aae-4db0-ab28-9dfea47bb57e)


## **2. 독립 실행형 애플리케이션**

- JAR 파일 하나로 실행 가능 (`java -jar app.jar`)
- 따라서 DevOps 및 클라우드 환경에서 배포가 용이
- 컨테이너(Docker) 환경과의 높은 호환성

## **3. MSA에 적합**

- RESTful API 개발 용이
- Spring Cloud와 연계하여 마이크로서비스 아키텍처 구현 가능
- 경량화된 실행 환경으로 확장성 및 유지보수 용이

## **4. 다양한 스타터 패키지 제공**

- `spring-boot-starter-web`, `spring-boot-starter-data-jpa`, `spring-boot-starter-security` 등 다양한 의존성 제공
- 특정 기능을 손쉽게 추가 가능

## **5. 강력한 모니터링 및 관리 기능**

- Spring Boot Actuator를 이용한 애플리케이션 모니터링 및 헬스 체크
- Prometheus, Grafana 등의 모니터링 도구와 연계 가능

# Spring Boot 애플리케이션의 실행 흐름

1. **OS 위에서 JVM 실행** → `java -jar app.jar`
2. **SpringApplication.run() 호출** → Spring Boot 애플리케이션 초기화
3. **내장 WAS(Tomcat) 실행** → HTTP 요청을 처리할 준비 완료
4. **ApplicationContext(= 스프링 컨테이너) 생성** → 빈(Bean) 등록 및 의존성 주입 진행
5. **서블릿 컨테이너에 필터, 서블릿 등록**
6. **DispatcherServlet 초기화** → HTTP 요청을 처리할 준비 완료
7. **비즈니스 로직 실행** → 요청 처리 및 응답 반환

# 구조

![image 1](https://github.com/user-attachments/assets/65587b54-9caf-427b-9a29-60f8503804cf)

### WS(Web Server)

- **네트워크 처리(HTTP 또는 Socket)**
    - 클라이언트가 요청을 보내면 해당 요청을 수락하기 위한 connection을 생성한다. 클라이언트와 서버 간의 통로를 만드는 과정이라고 할 수 있다.
    - 클라이언트의 요청을 받은 순간부터 응답을 전송하는 순간까지 전반적인 **네트워크 로직을 책임지고 처리**한다. 즉 데이터 송수신, 오류 처리, 보안 등 네트워크 통신에 필요한 모든 과정을 관리한다.
- **리소스 처리**
    - 클라이언트가 요청한 리소스가 **"정적"** 리소스(HTML, CSS, JS)인 경우, 별도의 처리 없이 즉시 해당 파일을 찾아 클라이언트에게 응답한다. 따라서 정적인 콘텐츠를 빠르고 효율적으로 제공할 수 있다.
    - 그러나 **"동적"** 리소스(DB 연동, 서버 측 로직 실행 등이 필요)인 경우, 해당 작업을 Servlet Container의 Task Queue에 전달한다. 동적 처리가 필요한 요청을 적절하게 분배하여 전체 시스템의 효율성을 높인다.

### Servlet Container(Web Container)

- **Servlet 관리 및 처리**
    - "동적"인 페이지가 필요한 경우 이를 생성하고 처리한다.
    - **Servlet**: Java 코드를 사용하여 동적인 콘텐츠를 생성하는 데 사용되는 핵심적인 구성 요소.
    - Servlet Container는 이러한 Servlet의 생명주기를 관리하고, 클라이언트 요청에 따라 적절한 Servlet을 실행하여 동적인 응답을 생성한다.
- **Thread Pool 관리**
    - **요청과 스레드가 1:1로 매핑**된다.
    - Task Queue에서 작업을 가져와 Thread Pool 내의 스레드에 할당하고, 해당 스레드를 통해 작업을 실행한다.
    - 이로 인해 개발자가 직접 멀티 스레드 코드를 작성하지 않아도 **스프링 부트 서버는 멀티 스레드 방식으로 동작**할 수 있게 된다. 이는 개발 생산성을 높이고, 복잡한 멀티 스레드 프로그래밍의 어려움을 덜어주는 스프링 부트의 중요한 장점이다.
- **Filter**
    - **요청/응답 필터링:** 요청 헤더 검사, 특정 IP 주소 접근 차단 등
    - **보안 강화:** 인증(Authentication) 및 권한 부여(Authorization) 등
    - **공통 로직 처리**: 로깅, 인코딩 변환, 프로파일링 등
- **Dispatcher Servlet**
    - 스프링 MVC의 핵심 컴포넌트로서, 클라이언트의 모든 요청을 가장 먼저 받아 처리하는 **Front Controller** 역할을 수행한다.
    - Spring이 제공하지만, Spring Boot를 사용하면 자동으로 **내장 WAS 내부의 Servlet Container에 등록**되어 실행된다.
    - 클라이언트의 요청을 분석하여 적절한 Handler(= Controller)에게 전달하고, 그 처리 결과를 View를 통해 클라이언트에게 응답하는 전체 과정을 관리한다.
    - 스프링 MVC의 중심에서 요청 처리 흐름을 제어하고, 웹 애플리케이션의 구조를 단순화하는 데 기여한다.

# 예상 면접 질문

### **1. Spring Boot와 Spring Framework의 차이점은 무엇인가?**

- Spring Framework는 애플리케이션 개발을 위한 종합적인 프레임워크이며, 설정이 복잡하다.
- Spring Boot는 Spring Framework 기반이지만, 자동 설정(Auto Configuration)과 내장 WAS를 제공하여 개발을 단순화한다.

### **2. Spring Boot의 주요 장점은 무엇인가?**

- 설정이 간소화되어 빠른 애플리케이션 개발이 가능하며, 자동 설정을 지원한다.
- 내장 WAS, 의존성 관리, 프로덕션 레디 기능(Actuator 등)을 제공하여 운영 편의성이 높다.

### **3. Spring Boot에서 내장 WAS를 사용하면 어떤 장점이 있는가?**

- 별도의 WAS 설치 없이 독립 실행형 애플리케이션으로 배포가 가능하여 배포 및 운영이 간편하다.
- 환경 간 일관성을 유지할 수 있으며, 컨테이너 오케스트레이션(Docker, Kubernetes)과의 연동이 용이하다.

### 4. WAS vs WS

- **WAS(Web Application Server)**: 웹 애플리케이션을 실행하는 서버로, 서블릿 컨테이너를 포함하며 동적 컨텐츠(JSP, Servlet 등)를 처리한다.
- **WS(Web Server)**: 정적인 컨텐츠(HTML, CSS, 이미지 등)를 제공하며, 요청을 WAS 또는 다른 서버로 전달하는 역할을 한다.

### **5. Spring Boot 애플리케이션의 초기화 과정을 설명하라.**

- 스프링 컨테이너(ApplicationContext)를 생성하고, 자동 설정을 적용하며, 내장 WAS를 실행한다.
- `CommandLineRunner` 실행 등 후처리 작업을 수행한 후, 최종적으로 애플리케이션을 실행 상태로 만든다.

### **6. Spring Boot에서 RESTful API를 어떻게 구현하는가?**

- `@RestController`와 `@RequestMapping`을 사용하여 REST 엔드포인트를 정의한다.
- `@GetMapping`, `@PostMapping` 등을 활용하여 HTTP 메서드별 요청을 처리하고, JSON 형식의 응답을 반환한다.

---

# [참고] Spring + 디자인 패턴

## **1. 싱글톤 패턴**

- Spring의 빈(Bean)은 기본적으로 싱글톤으로 관리됨 (`@Component`, `@Service`, `@Repository` 등)

![image 2](https://github.com/user-attachments/assets/54ecd069-d219-436b-ba31-95ce48b99a60)

![image 3](https://github.com/user-attachments/assets/7b0c6904-4b0c-40a3-9df9-3760c7c49cd0)

## **2. 팩토리 패턴**

- 스프링 컨테이너, 즉 `ApplicationContext`(BeanFactory를 상속)가 Bean 객체 생성 로직을 캡슐화하여 관리
- `@Bean`을 활용하여 객체를 직접 생성 가능

## **3. 프록시 패턴**

- Spring AOP에서 사용됨 (`@Transactional`, `@Cacheable` 등)
- 인터셉터 역할을 수행하여 부가기능(로깅, 트랜잭션 관리 등) 제공

![image 4](https://github.com/user-attachments/assets/95fe85fa-6052-4660-a7a6-bd116f54340f)

## **4. MVC 패턴**

- Spring Boot의 기본 구조 (`Controller-Service-Repository`)
- `DispatcherServlet`을 통한 요청 처리
- vs **Spring WebFlux**
    
    ![image 5](https://github.com/user-attachments/assets/b8f6581a-0e02-4e0c-8ca1-f8c2bff9fec4)

    ![image 6](https://github.com/user-attachments/assets/e6370aa5-591c-4350-ae7b-854aad2028cf)


---

# [참고] Spring + 네트워크

## **1. 내장 WAS와 HTTP 처리**

- Tomcat이 기본 내장 WAS로 사용됨 (`server.port=8080` 설정 가능)
- HTTP 요청을 DispatcherServlet이 처리하고 컨트롤러로 전달

## **2. RESTful API**

- `@RestController`, `@RequestMapping`을 사용하여 REST API 개발
- JSON 데이터 직렬화 및 역직렬화 처리

## **3. 로드 밸런싱 및 API Gateway**

- Spring Cloud Gateway를 사용하여 API Gateway 구현 가능
- Netflix Eureka, Ribbon, Feign과 연계하여 로드 밸런싱 가능

## **4. 캐싱 & 메시징**

- Redis, EhCache 등을 이용한 캐싱
- Kafka, RabbitMQ를 활용한 비동기 메시징 큐 연동

---

# [참고] Spring + 운영체제

## **1. 멀티 스레드**

- Spring Boot의 Servlet Container가 제공하는 ThreadPool을 통해 멀티스레드 환경에서 비동기 방식으로 동작
- 트래픽이 급증해 ThreadPool이 과부하된 경우, 최대 thread 수를 조정해야함 (기본값=200이며, application.yml에서 변경 가능)

## **2. 메모리 관리**

- JVM의 Heap과 Stack 메모리 관리 중요
- GC(Garbage Collection) 튜닝 필요 (G1GC, ZGC 등 활용 가능)

## **3. 파일 시스템과 로깅**

- Spring Boot 애플리케이션에서 로그 파일을 저장하고 관리 (`/logs` 디렉토리 활용)
- Logback, ELK Stack과 연계하여 로그 분석 가능

## **4. 컨테이너 환경 (Docker, Kubernetes)**

- Spring Boot 애플리케이션을 Docker 컨테이너로 배포 가능
- Kubernetes를 활용하여 스케일링 및 운영 자동화 가능

---

# 정리

- Spring Boot는 빠른 개발, 내장 WAS, 강력한 모니터링 기능 등을 제공하여 모던 애플리케이션 개발에 적합하다.
- 또한, 네트워크 및 운영체제와의 긴밀한 연계를 통해 대규모 서비스 운영도 가능하다.
- 면접 대비를 위해 Spring Boot의 실행 흐름, Spring에 적용된 주요 패턴, 네트워크 및 운영체제 관련 지식을 체계적으로 이해하는 것이 중요하다.
