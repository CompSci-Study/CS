## **[Singleton 패턴 관련 3-depth 면접 질문 예시]**

### **1-depth: 기본 개념 확인**

> ✅ Q1. Singleton 패턴이 무엇인가요?
> 
> 
> ✅ **Q2. Singleton 패턴을 사용하면 어떤 장점과 단점이 있나요?**
> 
> ✅ **Q3. Singleton 패턴을 구현하는 방법에는 어떤 것이 있나요?**
> 

### **2-depth: 응용 및 코드 분석**

> ✅ Q4. 멀티스레드 환경에서 Singleton을 안전하게 구현하려면 어떻게 해야 하나요?
> 
> 
> ✅ **Q5. `synchronized`를 사용한 Singleton 구현 방식의 단점은 무엇인가요? 이를 해결하는 방법이 있나요?**
> 
> ✅ **Q6. Singleton 패턴과 DI(Dependency Injection)는 어떤 관계가 있을까요? Spring에서 Singleton을 어떻게 관리하나요?**
> 
> ✅ **Q7. Singleton 패턴이 GC(Garbage Collection)에서 메모리 관리에 어떤 영향을 미칠 수 있나요?**
> 

### **3-depth: 실제 적용 사례 및 확장**

> ✅ Q8. Singleton 패턴을 사용하지 않고 같은 효과를 얻을 수 있는 방법이 있을까요?
> 
> 
> ✅ **Q9. Singleton 패턴이 유효하지 않은 경우는 언제인가요? (예: Serverless 환경, 분산 시스템 등)**
> 
> ✅ **Q10. Spring에서는 Singleton을 직접 구현하지 않아도 되는데, 직접 구현해야 하는 경우는 언제인가요?**
> 
> ✅ **Q11. Singleton 객체를 역직렬화(Deserialization)할 때 객체가 여러 개 생성될 수 있는 문제를 어떻게 해결할 수 있나요?**
> 
> ✅ **Q12. Double-Checked Locking 방식으로 Singleton을 구현할 때, 메모리 가시성 문제(Instruction Reordering)를 방지하려면 어떻게 해야 하나요?**
> 

이런 식으로 디자인 패턴마다 **기본 개념 → 응용 → 실무 적용 및 한계**를 단계적으로 묻는 방식이 일반적이야.

특히 백엔드 실무 면접에서는 **멀티스레드 환경에서의 동작, GC와의 관계, DI와의 조합, 성능 최적화** 등을 깊게 묻는 경향이 있어.

Singleton 패턴 외에도 **Factory 패턴, Strategy 패턴, Proxy 패턴, Observer 패턴** 등에서 비슷한 방식으로 깊이 있는 질문을 받을 수 있으니 준비할 때 참고하면 좋아!

## **[Strategy 패턴 관련 3-depth 면접 질문 예시]**

### **1-depth: 기본 개념 확인**

> ✅ Q1. Strategy 패턴이 무엇인가요?
> 
> 
> ✅ **Q2. Strategy 패턴을 사용하면 얻을 수 있는 장점과 단점은 무엇인가요?**
> 
> ✅ **Q3. Strategy 패턴을 활용하면 어떤 문제를 해결할 수 있나요?**
> 
> ✅ **Q4. Strategy 패턴과 Template Method 패턴의 차이점은 무엇인가요?**
> 

---

### **2-depth: 응용 및 코드 분석**

> ✅ Q5. Java에서 Strategy 패턴을 코드로 구현해볼 수 있나요?
> 
> 
> ✅ **Q6. Strategy 패턴을 활용한 실제 사례(예: 결제 방식 선택, 이미지 압축 알고리즘 등)를 설명해 주세요.**
> 
> ✅ **Q7. Strategy 패턴에서 전략(Strategy) 객체를 동적으로 변경할 수 있는 방법은 무엇인가요?**
> 
> ✅ **Q8. Strategy 패턴을 Singleton과 함께 사용하면 어떤 장점과 단점이 있을까요?**
> 
> ✅ **Q9. Strategy 패턴을 Spring에서 활용하는 방법을 설명할 수 있나요? (예: `@Component`와 `@Autowired`를 활용한 전략 주입)**
> 
> ✅ **Q10. 전략 객체를 Bean으로 관리하면 어떤 이점이 있을까요?**
> 

---

### **3-depth: 실제 적용 사례 및 확장**

> ✅ Q11. Strategy 패턴을 적용한 코드에서 새 전략을 추가해야 할 때 OCP(개방-폐쇄 원칙)를 어떻게 지킬 수 있을까요?
> 
> 
> ✅ **Q12. Strategy 패턴을 사용하지 않고 같은 효과를 얻을 수 있는 방법이 있을까요?**
> 
> ✅ **Q13. Spring의 `@Primary` 또는 `@Qualifier`를 사용하여 특정 전략을 선택하는 방식에 대해 설명해 주세요.**
> 
> ✅ **Q14. Strategy 패턴을 잘못 사용하면 발생할 수 있는 문제는 무엇인가요? (예: 지나치게 많은 객체 생성, 유지보수 어려움 등)**
> 
> ✅ **Q15. Strategy 패턴을 적용했지만 성능이 저하되는 경우 어떤 최적화 방법을 고려할 수 있나요?**
> 
> ✅ **Q16. Spring에서 Strategy 패턴을 `Map<String, Strategy>` 형태로 관리하면 어떤 장점이 있나요?**
> 
> ✅ **Q17. Strategy 패턴이 아닌 함수형 인터페이스(Functional Interface)나 람다 표현식을 활용하면 어떤 장점이 있나요?**
> 

---

### **📌 Strategy 패턴 면접 준비 TIP**

- **기본 개념 + 코드 구현**은 필수!
- **Spring DI, Bean 관리와 연계**해서 설명할 수 있으면 가산점 👍
- **확장성과 유지보수성**을 강조하면서 **OCP (개방-폐쇄 원칙)와의 관계**도 설명할 수 있도록 준비
- **람다 표현식, `Function<T, R>` 같은 자바의 함수형 프로그래밍 요소와 비교** 가능하면 더욱 강점

## **[Factory 패턴 관련 3-depth 면접 질문 예시]**

### **1-depth: 기본 개념 확인**

> ✅ Q1. Factory 패턴이란 무엇인가요?
> 
> 
> ✅ **Q2. Factory 패턴을 사용하면 얻을 수 있는 장점과 단점은 무엇인가요?**
> 
> ✅ **Q3. Factory 패턴과 생성자 호출(`new`)을 직접 사용하는 방식의 차이는 무엇인가요?**
> 
> ✅ **Q4. Factory 패턴과 Abstract Factory 패턴의 차이점은 무엇인가요?**
> 

---

### **2-depth: 응용 및 코드 분석**

> ✅ Q5. Java에서 Factory 패턴을 코드로 구현해볼 수 있나요?
> 
> 
> ✅ **Q6. Factory 패턴을 적용하면 OCP(개방-폐쇄 원칙)를 어떻게 지킬 수 있나요?**
> 
> ✅ **Q7. Factory 패턴에서 Enum을 활용하면 어떤 장점이 있을까요?**
> 
> ✅ **Q8. Spring에서는 Factory 패턴을 어떻게 활용할 수 있나요? (`@Bean`, `@Component`, `@Service` 등을 활용한 DI 연계)**
> 
> ✅ **Q9. Factory 패턴을 사용하지 않고 같은 효과를 얻을 수 있는 방법이 있을까요? (예: 리플렉션, 람다식, Map 활용 등)**
> 
> ✅ **Q10. Factory 패턴을 적용할 때 의존성 주입(DI)과의 관계는 어떤가요?**
> 

---

### **3-depth: 실제 적용 사례 및 확장**

> ✅ Q11. Factory 패턴을 잘못 사용하면 발생할 수 있는 문제는 무엇인가요?
> 
> 
> ✅ **Q12. Factory 패턴을 적용했는데 유지보수가 어려워지는 경우, 어떤 해결 방법을 고려할 수 있을까요?**
> 
> ✅ **Q13. Spring의 `FactoryBean<T>` 인터페이스는 무엇이고, 어떻게 활용할 수 있나요?**
> 
> ✅ **Q14. Factory 패턴을 멀티스레드 환경에서 사용할 때 주의해야 할 점은 무엇인가요?**
> 
> ✅ **Q15. Factory 패턴을 적용한 코드에서 객체 생성을 지연(예: Lazy Initialization)하려면 어떻게 해야 하나요?**
> 
> ✅ **Q16. Spring에서 Bean을 생성하는 방식과 Factory 패턴을 활용한 객체 생성 방식의 차이점은 무엇인가요?**
> 
> ✅ **Q17. Factory 패턴과 Prototype 패턴을 비교하면 어떤 차이가 있을까요?**
> 
> ✅ **Q18. Factory 패턴을 Reflection API와 함께 사용하면 어떤 장점과 단점이 있을까요?**
> 

---

### **📌 Factory 패턴 면접 준비 TIP**

- **기본 개념 + 코드 구현**은 필수!
- **OCP와 의존성 주입(DI)과의 관계**를 설명할 수 있어야 함
- **Factory 패턴을 Spring의 `@Bean`, `FactoryBean<T>`와 연계해서 설명**할 수 있으면 실무 면접에서 강점
- **Reflection, Enum, Map을 활용한 Factory 패턴의 변형**을 알고 있으면 플러스

## **[Observer 패턴 관련 3-depth 면접 질문 예시]**

### **1-depth: 기본 개념 확인**

> ✅ Q1. Observer 패턴이란 무엇인가요?
> 
> 
> ✅ **Q2. Observer 패턴을 사용하면 어떤 장점과 단점이 있나요?**
> 
> ✅ **Q3. Observer 패턴을 사용해야 하는 상황을 예로 들어 설명해 주세요.**
> 
> ✅ **Q4. Observer 패턴과 Publish-Subscribe 패턴의 차이점은 무엇인가요?**
> 

---

### **2-depth: 응용 및 코드 분석**

> ✅ Q5. Java에서 Observer 패턴을 코드로 구현해볼 수 있나요?
> 
> 
> ✅ **Q6. Observer 패턴에서 Observer 객체를 동적으로 추가하거나 제거할 수 있는 방법은 무엇인가요?**
> 
> ✅ **Q7. Observer 패턴을 활용한 실제 사례(예: 이벤트 리스너, 실시간 주가 알림 시스템 등)를 설명해 주세요.**
> 
> ✅ **Q8. Observer 패턴에서 순환 참조(Circular Dependency)가 발생할 가능성이 있나요? 있다면 어떻게 해결할 수 있을까요?**
> 
> ✅ **Q9. Java에서 `java.util.Observable`과 `java.util.Observer`가 deprecated된 이유는 무엇인가요?**
> 
> ✅ **Q10. Spring에서는 Observer 패턴을 어떻게 활용할 수 있나요? (`ApplicationEvent`, `@EventListener`)**
> 
> ✅ **Q11. Observer 패턴에서 이벤트가 많아지면 성능 저하가 발생할 수 있는데, 이를 최적화할 수 있는 방법은 무엇인가요?**
> 

---

### **3-depth: 실제 적용 사례 및 확장**

> ✅ Q12. Observer 패턴이 잘못 사용될 경우 발생할 수 있는 문제점은 무엇인가요?
> 
> 
> ✅ **Q13. Java에서 Reactive Programming (`Project Reactor`, `RxJava`)과 Observer 패턴의 관계는 무엇인가요?**
> 
> ✅ **Q14. Observer 패턴을 멀티스레드 환경에서 사용할 때 발생할 수 있는 문제와 해결 방법은 무엇인가요?**
> 
> ✅ **Q15. Observer 패턴에서 `pull` 방식과 `push` 방식의 차이점은 무엇인가요?**
> 
> ✅ **Q16. Observer 패턴과 MVC(Model-View-Controller) 패턴의 관계는 무엇인가요?**
> 
> ✅ **Q17. Observer 패턴과 Mediator 패턴을 비교하면 어떤 차이점이 있나요?**
> 
> ✅ **Q18. 대량의 Observer가 등록될 경우 메모리 관리 및 성능 최적화를 위해 어떤 기법을 사용할 수 있나요? (예: WeakReference 활용, 이벤트 큐 적용 등)**
> 
> ✅ **Q19. Event-driven Architecture(EDA)와 Observer 패턴의 관계를 설명할 수 있나요?**
> 

---

### **📌 Observer 패턴 면접 준비 TIP**

- **기본 개념 + 코드 구현**은 필수!
- **Spring의 `ApplicationEvent`, `@EventListener`와 연계**해서 설명하면 실무에서 강점
- **비동기 이벤트 처리, Reactive Programming (`Reactor`, `RxJava`)와의 관계**를 설명할 수 있으면 좋음
- **성능 최적화(메모리 누수, 멀티스레드 안전성, 이벤트 큐 적용 등)에 대한 대책**을 알고 있으면 더욱 강점

## **[프록시 서버와 프록시 패턴 관련 3-depth 면접 질문 예시]**

### **📌 1-depth: 기본 개념 확인**

> ✅ Q1. 프록시(Proxy) 서버란 무엇인가요?
> 
> 
> ✅ **Q2. 프록시 패턴(Proxy Pattern)이란 무엇인가요?**
> 
> ✅ **Q3. 프록시 패턴의 주요 목적과 장점, 단점은 무엇인가요?**
> 
> ✅ **Q4. 프록시 패턴과 데코레이터(Decorator) 패턴의 차이점은 무엇인가요?**
> 
> ✅ **Q5. 프록시 서버의 주요 역할(예: 캐싱, 보안, 로드 밸런싱 등)은 무엇인가요?**
> 
> ✅ **Q6. 프록시 서버와 게이트웨이(Gateway)의 차이점은 무엇인가요?**
> 
> ✅ **Q7. 정적 프록시(Static Proxy)와 동적 프록시(Dynamic Proxy)의 차이는 무엇인가요?**
> 

---

### **📌 2-depth: 응용 및 코드 분석**

> ✅ Q8. Java에서 정적 프록시를 코드로 구현해볼 수 있나요?
> 
> 
> ✅ **Q9. Java의 `Proxy` 클래스와 리플렉션(Reflection)을 사용하여 동적 프록시를 구현하는 방법을 설명해주세요.**
> 
> ✅ **Q10. CGLIB을 활용한 프록시와 JDK Dynamic Proxy의 차이점은 무엇인가요?**
> 
> ✅ **Q11. Spring에서 AOP(Aspect-Oriented Programming)와 프록시 패턴의 관계는 무엇인가요?**
> 
> ✅ **Q12. 프록시 패턴을 이용한 로깅, 트랜잭션 관리, 성능 모니터링 구현 방법을 설명할 수 있나요?**
> 
> ✅ **Q13. 가상 프록시(Virtual Proxy), 보호 프록시(Protection Proxy), 캐싱 프록시(Caching Proxy)의 차이점은 무엇인가요?**
> 
> ✅ **Q14. HTTP 프록시 서버(Forward Proxy)와 리버스 프록시(Reverse Proxy)의 차이점은 무엇인가요?**
> 
> ✅ **Q15. Spring의 `@Transactional`은 내부적으로 어떤 방식으로 프록시를 생성하나요?**
> 

---

### **📌 3-depth: 실제 적용 사례 및 확장**

> ✅ Q16. 프록시 서버를 활용하여 CDN(Content Delivery Network)을 구축하는 원리를 설명할 수 있나요?
> 
> 
> ✅ **Q17. 프록시 패턴을 잘못 사용하면 발생할 수 있는 문제는 무엇인가요?**
> 
> ✅ **Q18. Spring에서 AOP 기반 프록시 방식과 ByteBuddy, CGLIB 기반 프록시 방식의 차이점은 무엇인가요?**
> 
> ✅ **Q19. JPA의 Lazy Loading은 프록시 패턴을 어떻게 활용하나요?**
> 
> ✅ **Q20. 프록시 패턴을 사용하지 않고 같은 효과를 얻을 수 있는 방법이 있을까요?**
> 
> ✅ **Q21. 프록시 서버를 사용한 네트워크 보안 강화 기법에 대해 설명할 수 있나요?**
> 
> ✅ **Q22. 프록시 서버를 통한 SSL/TLS 암호화 통신 방식(예: MITM Proxy)을 설명할 수 있나요?**
> 
> ✅ **Q23. Kubernetes에서 프록시 패턴이 적용된 대표적인 기술(Nginx Ingress Controller, Envoy 등)은 무엇인가요?**
> 
> ✅ **Q24. Spring Cloud Gateway와 프록시 서버의 차이점은 무엇인가요?**
> 

---

### **📌 프록시 관련 면접 준비 TIP**

- **프록시 서버와 프록시 패턴을 명확히 구분해서 설명할 것**
- **Spring AOP, JPA Lazy Loading, 트랜잭션 처리에서의 프록시 활용 방법을 이해할 것**
- **동적 프록시와 정적 프록시의 차이점을 코드로 설명 가능할 것**
- **HTTP 프록시 서버(Forward Proxy) vs. 리버스 프록시(Reverse Proxy) 차이점 설명 가능할 것**
- **CDN, API Gateway, Nginx 등 실무 활용 사례를 알면 가산점**
- **보안적인 측면(MITM Proxy, SSL/TLS 암호화 우회 등)에 대해서도 기본적인 개념을 알고 있으면 좋음**

## **[Decorator 패턴 관련 3-depth 면접 질문 예시]**

### **📌 1-depth: 기본 개념 확인**

> ✅ Q1. 데코레이터(Decorator) 패턴이란 무엇인가요?
> 
> 
> ✅ **Q2. 데코레이터 패턴을 사용하면 어떤 장점과 단점이 있나요?**
> 
> ✅ **Q3. 데코레이터 패턴과 상속(Inheritance)의 차이점은 무엇인가요?**
> 
> ✅ **Q4. 데코레이터 패턴을 활용하면 어떤 문제를 해결할 수 있나요?**
> 
> ✅ **Q5. 데코레이터 패턴과 프록시 패턴의 차이점은 무엇인가요?**
> 
> ✅ **Q6. 데코레이터 패턴과 전략(Strategy) 패턴의 차이점은 무엇인가요?**
> 

---

### **📌 2-depth: 응용 및 코드 분석**

> ✅ Q7. Java에서 데코레이터 패턴을 코드로 구현해볼 수 있나요?
> 
> 
> ✅ **Q8. Java의 `InputStream`, `OutputStream`이 데코레이터 패턴을 어떻게 활용하고 있나요?**
> 
> ✅ **Q9. 데코레이터 패턴에서 기존 객체에 데코레이션을 동적으로 추가하는 방법을 설명할 수 있나요?**
> 
> ✅ **Q10. 데코레이터 패턴에서 중첩된 데코레이터가 많아질 경우 성능 이슈는 어떻게 해결할 수 있나요?**
> 
> ✅ **Q11. 데코레이터 패턴을 Spring에서 활용하는 방법을 설명할 수 있나요? (`BeanPostProcessor`, `AOP`)**
> 
> ✅ **Q12. 데코레이터 패턴을 사용한 실제 사례(예: 로깅 시스템, 캐싱, 보안 필터 등)를 설명해 주세요.**
> 
> ✅ **Q13. 데코레이터 패턴을 팩토리 패턴과 결합하여 사용할 수 있을까요?**
> 

---

### **📌 3-depth: 실제 적용 사례 및 확장**

> ✅ Q14. 데코레이터 패턴이 잘못 사용되면 발생할 수 있는 문제점은 무엇인가요?
> 
> 
> ✅ **Q15. 데코레이터 패턴을 Spring의 `@Aspect`를 활용하여 구현할 수 있나요?**
> 
> ✅ **Q16. 데코레이터 패턴을 사용하지 않고 같은 효과를 얻을 수 있는 방법이 있을까요?**
> 
> ✅ **Q17. 데코레이터 패턴이 과도하게 사용될 경우 유지보수성이 떨어지는 이유는 무엇인가요?**
> 
> ✅ **Q18. 데코레이터 패턴을 멀티스레드 환경에서 사용할 때 주의해야 할 점은 무엇인가요?**
> 
> ✅ **Q19. 데코레이터 패턴과 `Proxy` 패턴을 조합하여 사용할 수 있는 사례가 있을까요?**
> 
> ✅ **Q20. 데코레이터 패턴을 Lambda(람다) 표현식으로 대체할 수 있을까요?**
> 
> ✅ **Q21. 데코레이터 패턴과 Observer 패턴을 함께 사용할 수 있는 사례는 무엇인가요?**
> 
> ✅ **Q22. Java의 `BufferedReader`, `DataInputStream`이 데코레이터 패턴을 적용한 사례로 볼 수 있는 이유는 무엇인가요?**
> 
> ✅ **Q23. 프레임워크(예: Spring Boot)에서 데코레이터 패턴을 사용하는 대표적인 예시는 무엇인가요?**
> 

---

### **📌 데코레이터 패턴 면접 준비 TIP**

- **기본 개념 + 코드 구현**은 필수!
- **자바 표준 라이브러리(`InputStream`, `BufferedReader`, `DataInputStream`)에서 데코레이터 패턴 적용 사례**를 알고 있으면 유리
- **Spring AOP(`@Aspect`), `BeanPostProcessor`와의 관계**를 이해하고 있으면 강점
- **데코레이터 패턴이 과도할 경우 발생하는 유지보수 문제**도 함께 고려할 것
- **람다식, 함수형 인터페이스(`Function<T, R>`)를 활용한 데코레이터 패턴 대체 방법**도 준비하면 플러스

## 실무에서 많이 활용되는 **핵심 디자인 패턴 7가지**

| 패턴 이름 | 주요 목적 | 실무 적용 사례 |
| --- | --- | --- |
| Singleton (싱글톤) | 동일 객체를 공유하여 리소스 절약 | Spring Bean, DB 커넥션 풀 |
| Factory (팩토리) | 객체 생성을 중앙에서 관리 | Service Factory, Cloud Provider |
| Proxy (프록시) | 접근 제어, 로드 밸런싱, 성능 최적화 | Spring AOP, API Gateway |
| Flyweight (플라이웨이트) | 객체 공유로 메모리 사용 절약 | 캐싱, DB 커넥션 풀 |
| Circuit Breaker (서킷 브레이커) | 장애 전파 방지 | Resilience4j, Netflix Hystrix |
| Event-Driven (이벤트 기반) | 비동기 처리, 확장성 향상 | Kafka, RabbitMQ |
| CQRS (Command Query Responsibility Segregation) | 읽기/쓰기 성능 최적화 | MongoDB + MySQL 조합 |

## **1. Singleton 패턴**

### ✅ 대규모 트래픽 대응 방법

- **객체를 공유해 메모리 사용을 줄이고 GC(가비지 컬렉션) 부담 감소**
- **DB Connection Pool (HikariCP), Thread Pool, 캐싱 시스템에서 필수적**
- Spring의 **Bean Scope**에서 `@Singleton` 기본 적용

### 🚀 **적용 사례**

- Spring Container의 Bean 관리
- MySQL Connection Pool
- Redis를 활용한 캐싱 (세션 공유)

---

## **2. Factory 패턴**

### ✅ 유지보수성과 확장성 고려

- **객체 생성 로직을 캡슐화하여 관리** → 새로운 서비스가 추가되더라도 Factory만 수정하면 됨
- **객체 생성을 중앙에서 관리** → 특정 로직에 따라 객체를 동적으로 생성 가능

### 🚀 **적용 사례**

- `ServiceFactory`를 이용한 Service 객체 생성
- 클라우드 환경에서 특정 **Cloud Provider (AWS, GCP, Azure)** 에 따라 구현 클래스 분리

---

## **3. Proxy 패턴**

### ✅ 성능 최적화 & 보안

- **프록시 서버를 통해 부하를 분산 & 캐싱 적용 가능**
- **Spring AOP 기반 트랜잭션 관리 (`@Transactional`)**
- **API Gateway (Nginx, Kong) 에서 Rate Limiting 적용**

### 🚀 **적용 사례**

- `@Transactional`을 활용한 DB 트랜잭션 관리
- Redis/Memcached를 활용한 API 응답 캐싱
- API Gateway (Netflix Zuul, Kong)

---

## **4. Flyweight 패턴**

### ✅ 대규모 트래픽 처리 & 성능 최적화

- **자주 사용되는 객체를 공유해 메모리 사용 절감**
- **대량의 반복 객체(예: 웹 브라우저 폰트 렌더링, DB 커넥션 공유)에서 효과적**
- 캐싱 시스템 (Redis, LRU Cache)와 연계 가능

### 🚀 **적용 사례**

- **MySQL Connection Pool, HTTP Client Pool**
- **LRU Cache** 기반 API 응답 캐싱
- **Spring Bean Scope (`@Component`)로 Singleton 관리**

---

## **5. Circuit Breaker 패턴**

### ✅ 장애 전파 방지 & 회복력 강화

- **하위 시스템 장애 시 서비스 전체가 다운되는 것을 방지**
- Netflix의 **Hystrix**, Spring의 **Resilience4j** 라이브러리를 활용

### 🚀 **적용 사례**

- **API Gateway에서 특정 서비스 응답 시간 초과 시 우회 (Fallback)**
- **외부 API 요청 실패 시 대체 응답을 제공하는 패턴**
- **마이크로서비스에서 특정 서비스 장애 시 전체 장애를 방지**

---

## **6. Event-Driven 패턴**

### ✅ 확장성과 비동기 처리

- **메시지 큐(Kafka, RabbitMQ) 기반 비동기 이벤트 처리**
- **비동기 이벤트 방식으로 성능 향상**
- **데이터 처리 속도를 향상시켜 대량 트래픽 대응**

### 🚀 **적용 사례**

- **Kafka 기반 로그 수집 (Elastic Stack 연계)**
- **이메일/SMS 알림 시스템 (RabbitMQ 사용)**
- **비동기 데이터 저장 (Kafka → Cassandra/Elasticsearch)**

---

## **7. CQRS (Command Query Responsibility Segregation)**

### ✅ 읽기/쓰기 분리로 성능 최적화

- **읽기(READ)와 쓰기(WRITE) 데이터 저장소를 분리하여 성능 개선**
- **MySQL + MongoDB, MySQL + Elasticsearch 조합 활용**
- **트랜잭션 부담을 줄이고, 검색 성능을 극대화**

### 🚀 **적용 사례**

- **MySQL(쓰기) + Elasticsearch(읽기) 기반 검색 서비스**
- **MongoDB(읽기) + MySQL(쓰기) 조합으로 읽기 성능 최적화**
- **이커머스 주문 시스템 (주문 정보는 RDB, 검색은 NoSQL 활용)**

## **💡 면접 TIP**

1. **단순 개념 설명이 아니라 실무 적용 사례까지 준비할 것**→ "이 패턴을 언제, 왜, 어떻게 사용하나요?"라는 질문이 나올 가능성이 큼
2. **Spring 및 대규모 트래픽 대응 기술과 연결할 것**→ Spring AOP, Kafka, Redis, API Gateway 등과 연계한 답변이 중요
3. **단점 & 해결 방법도 언급할 것**→ 예: "Singleton은 멀티스레드에서 동기화 문제가 발생할 수 있음 → 해결책: `Double-Checked Locking` 사용"