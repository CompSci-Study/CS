# 주요 기능 4가지

- Authentication
- Authorization
- Protection against Exploits
- Integrations

## Authentication (인증)

- 유저의 신원을 확인하는 단계. 인증이 끝나면 인가 단계로 넘어간다

## Authorization(권한, 인가)

- 유저가 특정 자원에 접근할 권한이 있는지 확인하는 단계

## Protection against Exploits (취약점 보호)

- CSRF
    - 사이트에 인증, 인가가 완료된 사용자가 악의적인 사이트에 접속해 해당 정보를 사용해 악성 요청을 보내면 사이트가 이를 인가된 요청으로 인식해 공격당하는 것
    - 쿠키가 요청의 발송 위치와 관계없이 클라이언트 세션에 종속되어 같이 보내지기 때문
    - 해결방안
        
        공통) HTTP 요청은 read-only여야 함 → 요청은 서버의 상태를 변화시킬 수 없다 (ex. GET)
        
        1. POST RequestBody에 csrf 토큰을 넣어 보내는 것 (악성 사이트는 same-origin-policy 때문에 해당 request와 response를 볼 수 없다)
        2. Cookie에 SameSite 옵션 넣기
    - Content-Type을 지정하지 않고 데이터로 JSON을 사용할 경우 아래와 같은 취약점이 발생 가능하니 주의할 것
    
    
- HTTP Response Headers
    
    ```jsx
    <-- 기본적으로 제공되는 보안 옵션 -->
    Cache-Control: no-cache, no-store, max-age=0, must-revalidate
    Pragma: no-cache
    Expires: 0
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000 ; includeSubDomains
    X-Frame-Options: DENY
    X-XSS-Protection: 0
    ```
    
    - Content-Security-Policy, Referrer-Policy, 커스텀 헤더 등으로 추가적인 보안
- HTTP Requests
    - HTTP 요청이 올 경우 HTTPS 요청으로 처리하도록 리다이렉트

## Integrations (통합)

Cryptography, Spring Data, Java’s Concurrency APIs, Jackson, Localization 등

# 서블릿에서의 활용

## 기초적인 Security Configuration

```jsx
@EnableWebSecurity
@Configuration
public class DefaultSecurityConfig {
    @Bean
    @ConditionalOnMissingBean(UserDetailsService.class)
    InMemoryUserDetailsManager inMemoryUserDetailsManager() {
        String generatedPassword = // ...;
        return new InMemoryUserDetailsManager(User.withUsername("user")
                .password(generatedPassword).roles("USER").build());
    }

    @Bean
    @ConditionalOnMissingBean(AuthenticationEventPublisher.class)
    DefaultAuthenticationEventPublisher defaultAuthenticationEventPublisher(ApplicationEventPublisher delegate) {
        return new DefaultAuthenticationEventPublisher(delegate);
    }
}
```

# Spring Security의 구조

![기본 필터체인](https://github.com/user-attachments/assets/53061d67-a4f2-463b-ac45-6e79e414b355)

![image](https://github.com/user-attachments/assets/3008f772-6ae8-41c8-a0d1-519bc873ea65)


기본 필터체인 vs. DelegatingFilterProxy를 사용한 필터체인

- 기본적으로 필터체인은 서블릿에서 관리한다. 서블릿 컨테이너와 스프링의 Application Context (user defined)를 연결하는게 DelegatingFilterProxy
- DelegatingFilterProxy를 이용해 필터를 서블릿 컨테이너에 넣고 실질적인 작업은 Filter를 구현한 스프링 빈(@Bean)이 하게 하는 것
- 필터체인에서 DelegatingFilterProxy의 차례가 오면 구현된 Bean Filter0를 찾아 실행시킨다
- 서블릿 컨테이너는 실행 전 모든 Filter가 필요함, 하지만 스프링의 ContextLoaderListener를 이용한 Bean 로딩은 해당 시점보다 뒤이기 때문에 이런 절차가 필요

![image](https://github.com/user-attachments/assets/9723d97c-298a-484a-8531-10f1c2a9669e)


- Spring Security는 SecurityFilterChain을 통해 많은 필터를 실행하게 하는 FilterChainProxy라는 Bean을 DelegatingFilterProxy안에 넣어 프록시를 거쳐오면 여러 단계의 필터를 거치게 할 수 있다.

![image](https://github.com/user-attachments/assets/622d2084-d38e-4452-ad25-defd0e5b8d7f)


- FilterChainProxy는 SecurityFilterChain에서 어느 Filter가 실행되어야 하는지 결정한다.
- SecurityFilterChain 안의 SecurityFilter들은 (대체로) 모두 Bean이다.

![image](https://github.com/user-attachments/assets/81a57131-aa96-447c-86d9-838518002c2c)


- 위와 같이 FilterChainProxy가 여러 개의 SecurityFilterChain을 가질 수 있고, API endpoint로 사용할 체인을 결정지을 수 있다. 이 과정에서 RequestMatcher를 사용한다.
- 각각의 SecurityFilterChain은 독립적이며, 다양하게 define될 수 있다. (Bean 0개도 가능)
- [필터체인은 순서가 있다.](https://github.com/spring-projects/spring-security/blob/6.4.3/config/src/main/java/org/springframework/security/config/annotation/web/builders/FilterOrderRegistration.java) 이 중 필요한 필터를 골라서 사용하면 된다.

```jsx
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(Customizer.withDefaults())
            .httpBasic(Customizer.withDefaults())
            .formLogin(Customizer.withDefaults())
            .authorizeHttpRequests(authorize -> authorize
                .anyRequest().authenticated()
            );

        return http.build();
    }

}
```

- 위는 간단한 SecurityFilterChain의 예시로, csrf → httpBasic →  formLogin → authorization 필터 순으로 실행된다. (이것들만 실행되는 것은 아니다. SecurityFilterChain의 필수 필터 몇가지가 더 실행된다.)

## 커스텀 필터

Spring Security에서 제공하는 필터로 부족하다면, 직접 Filter Bean을 생성해 SecurityFilterChain에 삽입할 수 있다.

[HttpSecurity](https://docs.spring.io/spring-security/reference/api/java/org/springframework/security/config/annotation/web/builders/HttpSecurity.html) 클래스의 메서드를 이용한다.

- `#addFilterBefore(Filter, Class<?>)` : 특정 필터 전에 삽입
- `#addFilterAfter(Filter, Class<?>)` : 특정 필터 후에 삽입
- `#addFilterAt(Filter, Class<?>)` : 특정 필터 대신 삽입

- 필터 삽입 위치 고려사항(시점)
    1. SecurityContext가 로드된 시점
    2. CSRF, CORS 등 보안 필터들을 거친 시점
    3. 요청이 인증된 시점
    4. 요청이 인가된(권한이 주어진) 시점
    
    **위의 요소를 고려해서 필터의 위치를 정한다.**
    

### 예시

```jsx
import java.io.IOException;

import jakarta.servlet.Filter;
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import org.springframework.security.access.AccessDeniedException;

public class TenantFilter implements Filter {

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;

        String tenantId = request.getHeader("X-Tenant-Id"); (1)
        boolean hasAccess = isUserAllowed(tenantId); (2)
        if (hasAccess) {
            filterChain.doFilter(request, response); (3)
            return;
        }
        throw new AccessDeniedException("Access denied"); (4)
    }

}
```

위와 같이 로그인 한 유저의 권한을 확인하는 필터라면, 인증된 시점에 작성하는 것이 논리적이다.

구현 방식은 아래와 같다:

```jsx
@Bean
SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http
        // ...
        .addFilterAfter(new TenantFilter(), AnonymousAuthenticationFilter.class);
    return http.build();
}
```

**주의사항**:  커스텀 필터를 @Bean, @Component 등의 annotation으로 Spring bean으로 만들면, 해당 필터는 자동적으로 스프링 컨테이너에 종속되어 자동 실행될 수 있다. 총 2회 실행되는 셈이니 주의가 필요하다. (컨테이너에서 실행 → Spring Security에서 실행)

⇒ 그럼에도 Dependency Injection 등의 이유로 bean이 필요하다면, FilterRegistrationBean wrapper를 이용해 bean으로 만들고, 아래와 같은 설정을 통해 스프링 컨테이너에 등록하지 않도록 할 수 있다.

```jsx
@Bean
public FilterRegistrationBean<TenantFilter> tenantFilterRegistration(TenantFilter filter) {
    FilterRegistrationBean<TenantFilter> registration = new FilterRegistrationBean<>(filter);
    registration.setEnabled(false); <-- 이 부분!!!
    return registration;
}
```

Spring Security가 제공하는 Filter도 사용자가 정의해 쓸 수 있는데, 그런 경우 2번 호출하지 않도록 주의하자!

```jsx
@Bean
SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
	BasicAuthenticationFilter basic = new BasicAuthenticationFilter();
	// ... configure

	http
		.httpBasic(Customizer.withDefaults())
		// ... on no! BasicAuthenticationFilter is added twice!
		.addFilterAt(basic, BasicAuthenticationFilter.class);

	return http.build();
}
```

## 예외처리

![image](https://github.com/user-attachments/assets/89aac74c-d63a-4a76-99f8-8a4e9851890a)


```jsx
try {
	filterChain.doFilter(request, response);
} catch (AccessDeniedException | AuthenticationException ex) {
	if (!authenticated || ex instanceof AuthenticationException) {
		startAuthentication();
	} else {
		accessDenied();
	}
}
```

ExceptionTranslationFilter는 SecurityFilterChain에 등록된 필터로써 해당 SFC에 (등록된) 예외가 발생하면 필터체인이 오류없이 계속 동작하게 한다. 

- 해당 예외가 AuthenticationException인 경우 : 2번으로, 유저가 다시 인증하게끔 한다.
- 해당 예외가 AccessDeniedException인 경우 : 3번으로, 접속을 차단한다.
- 그 외의 오류 : ExceptionTranslationFilter의 구현에 따라 다르나, 오류 혹은 무시한다.

2번의 경우, 유저의 재인증이 끝난 후 기존 요청을 처리해야 하므로 RequestCache를 사용해 저장해 놓는다.

```jsx
@Bean
DefaultSecurityFilterChain springSecurity(HttpSecurity http) throws Exception {
	HttpSessionRequestCache requestCache = new HttpSessionRequestCache();
	requestCache.setMatchingRequestParameterName("continue");
	http
		// ...
		.requestCache((cache) -> cache
			.requestCache(requestCache)
		);
	return http.build();
}
```

위의 코드는 쿼리 파라미터로 continue=true가 전달되었을 경우에만 requestCache에 저장하고 사용하게 하는 방식이다. 이 경우 continue 파라미터가 없는 요청은 재인증 후에 자동 연결되지 않는다.

파라미터를 설정하지 않는 경우 requestCache는 모든 요청에 사용된다.

NullRequestCache를 이용해 캐시를 아예 사용하지 않는 방법도 있다.

저장한 캐시는 RequestCacheAwareFilter에서 사용된다.
