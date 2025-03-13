# Cache_&_Redis_이규빈

날짜: 2025년 3월 13일

# 1. Redis

### 개념

- REmote DIctionary Server: “외부”에 “딕셔너리”(key-value) 형태로 저장하는 “서버”
- **캐싱** 용도로 가장 많이 사용되지만, **임시작업 큐, 실시간 채팅, 메시지브로커** 등으로도 사용된다.

### 특징

![image.png](image.png)

![image.png](image%201.png)

### Persistence 옵션

- **RDB (Redis Database Backup)**
    
    일정 주기로 DB에 스냅샷을 저장하는 방식.
    
    장점: 압축해 저장하므로 크기가 작다. **“로딩/복구”** 속도가 빠르다.
    
    단점: 스냅샷 저장 전의 최신 데이터 유실 가능성이 있다.
    
    ![image.png](image%202.png)
    

- **AOF (Append Only File)**
    
    쓰기 작업시마다 로그 파일에 기록하는 방식.
    
    장점: 실시간으로 데이터 백업이 가능하므로 데이터 손실이 거의 없다. **“저장”** 속도가 빠르다. 
    
    단점: 명령 실행 기록을 모두 기록하므로 파일 크기가 크다. “로딩/복구” 속도가 느리다.
    
    ![image.png](image%203.png)
    

### Redis 사용시 주의사항⭐

- 데이터 타입에 따른 **적절한 자료구조**를 사용하라.
    
    ex. 중복을 허용하지 않고 최신순 조회만 하는 경우 → 애초에 Redis에 **SortedSet**으로 저장하기
    
- O(N) 시간복잡도를 가지는 명령어를 주의하라. (**∵ Single Thread)**
    
    ex. KEYS, FLUSHALL, FLUSHDB, Delete Collections, Get All Collections…
    
- **메모리 단편화** 문제가 발생할 수 있다. (**∵ 인메모리 DB)**
    
    → RSS(Resident Set Size = 실제 물리 메모리 사용량) 모니터링 필요.
    
- Redis를 사용하는 **목적을 분명히** 하라.
    
    Persistence 기능은 장애 발생 가능성이 높다. 
    
    → 데이터가 일부 유실되어도 문제가 없다면 Persistence 기능 OFF하는 것이 권장됨.
    

# 2. 캐싱 전략

레디스는 “인메모리” DB이기 때문에 **메모리 용량이 부족**하거나 **데이터가 휘발**될 위험이 있다.

이러한 특징 때문에 상황에 따른 적절한 전략 선정이 필요하다.

### 읽기 전략

- **Look Aside**
    
    장점: 캐시에 문제 발생시 DB로 요청을 위임할 수 있다.
    
    단점: 캐시와 DB의 데이터 정합성 유지가 어렵다. 첫 조회시 DB에 과부하가 발생한다.
    
    ![image.png](image%204.png)
    
- **Read Through**
    
    장점: 데이터 정합성이 강력히 보장된다.
    
    단점: 캐시가 다운되면 데이터 조회 자체가 불가능하다.
    
    ![image.png](image%205.png)
    

### 쓰기 전략

- **Write Around**
    
    장점: 성능이 좋다.
    
    단점: 캐시와 DB의 데이터 정합성 유지가 어렵다. (cache miss 발생시 비로소 캐시에 저장됨. 따라서 그 전까지는 데이터 불일치.)
    
    ![image.png](image%206.png)
    

- **Write Back**
    
    장점: 일정 주기마다 “묶어서” DB에 저장하므로, 쓰기 비용을 줄일 수 있다.
    
    단점: 캐시의 데이터 유실 위험이 있다.
    
    ![image.png](image%207.png)
    
- **Write Through**
    
    장점: 데이터 정합성이 강력히 보장된다.
    
    단점: 항상 2번씩 쓰게 되므로, 성능이 떨어질 수 있다.
    
    ![image.png](image%208.png)
    

# 3. Spring Data Redis

Spring에서 Redis를 쉽게 사용할 수 있도록 도와주는 라이브러리다. 즉 비유하자면 다음과 같다.

- 스프링 애플리케이션 = 운전자
- Redis = 자동차
- Spring Data Redis = 핸들, 액셀 등 자동차를 쉽게 운전하게 도와주는 인터페이스.

## Redis 연결 관리

Spring Data Redis는 Redis와의 연결을 관리하기 위해, `Lettuce` 같은 Redis client 라이브러리를 내부적으로 사용하고 있다.

- Jedis: 예전의 기본값이었으나, 멀티 스레드 불안정이나 pool 한계 등의 단점으로 인해 deprecated.
- **Lettuce (Spring Boot 기본값)**: Netty 기반의 비동기 Redis 클라이언트. 스레드 안전성이 뛰어나며, 비동기 및 반응형 프로그래밍을 지원한다.
- **Redisson**: 고급 분산 기능을 제공하는 Redis 클라이언트로, **분산 락**, 분산 캐시, 메시지 큐 등의 기능을 포함하고 있다.

## Spring Boot에서 Redis 설정 방법

1. **의존성 추가** (`build.gradle`)

```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-redis'
}
```

1. **Redis 연결 정보 설정** (`application.yml`)

```yaml
spring:
  redis:
    host: localhost
    port: 6379
    password: ''  # 비밀번호가 없으면 빈 문자열 유지
```

1. **RedisTemplate**

Redis 작업을 추상화한 클래스이다.

Spring Boot에서 Redis를 사용하려면 `RedisTemplate`을 빈으로 등록하여 사용할 수 있다. (문자열 처리에 특화된 `StringRedisTemplate`도 있음)

```java
// RedisTemplate 설정 예시

@Configuration
public class RedisConfig {

    @Value("${spring.redis.host}")
    private String host;

    @Value("${spring.redis.port}")
    private int port;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory(host, port);
    }

    @Bean
    public RedisTemplate<?, ?> redisTemplate() {
        RedisTemplate<?, ?> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory());
        return redisTemplate;
    }
}
```

```java
// Redis 사용 예시

@Service
public class RedisService {

    private final RedisTemplate<String, Object> redisTemplate;

    public RedisService(RedisTemplate<String, Object> redisTemplate) {
        this.redisTemplate = redisTemplate;
    }

    public void saveData(String key, Object value) {
        redisTemplate.opsForValue().set(key, value);
    }

    public Object getData(String key) {
        return redisTemplate.opsForValue().get(key);
    }
}

```

사용하려는 자료구조에 맞춰 opsForValue, opsForList, opsForSet, opsForHash, opsForZSet 등을 적절히 사용하기

# 참고

https://www.youtube.com/watch?v=tVZ15cCRAyE

https://www.youtube.com/watch?v=Gimv7hroM8A

https://www.youtube.com/watch?v=UkYdk1KKVCA&t=2s

https://bcp0109.tistory.com/328