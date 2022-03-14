# 목차

---
- [Spring Boot 환경 설정](#spring-boot------)
    * [1. 프로젝트 생성](#1--------)
    * [2. 라이브러리 살펴보기](#2-----------)
    * [3. view 환경설정](#3-view-----)
    * [4. 빌드 & 실행 (윈도우)](#4--------------)
- [회원 관리 예제](#--------)
    * [요구 사항](#-----)
- [스프링 웹 개발 기초](#-----------)
    * [1. 정적 컨텐츠](#1-------)
    * [2. MVC](#2-mvc)
    * [3. API](#3-api)
- [스프링 빈과 의존관계](#-----------)
    * [1. 컴포넌트 스캔과 자동 의존관계 설정](#1--------------------)
    * [2. 자바 코드로 직접 스프링 빈 등록하기](#2---------------------)
- [스프링 DB 접근 기술](#----db------)
    * [1. H2 데이터베이스 사용](#1-h2----------)
    * [2. Jdbc](#2-jdbc)
    * [3. Jdbc Template](#3-jdbc-template)
    * [4. JPA](#4-jpa)
    * [5. 스프링 데이터 JPA](#5---------jpa)
- [AOP](#aop)
    * [1. AOP가 필요한 상황](#1-aop--------)
    * [2. AOP 동작방식](#2-aop-----)
    
# Spring Boot 환경 설정

---

## 1. 프로젝트 생성

> 스프링 부트 프로젝트 생성

```
https://start.spring.io
```

> [build.gradle 파일]
>
> > 라이브러리 확인 가능
> >
> > > thymleaf : html 템플릿 엔진

```
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

## 2. 라이브러리 살펴보기

> gradle, mave은 의존관계가 있는 라이브러리를 자동으로 함께 다운로드 (스프링코어까지!)

### 스프링 부트 라이브러리

1. spring-boot-starter-web
   1. spring-boot-starter-tomcat: 톰캣 (웹서버)
   2. spring-webmvc: 스프링 웹 MVC
2. spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진(View)
3. spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅
   1. spring-boot
   - spring-core
   2. spring-boot-starter-logging
   - logback, slf4j

### 테스트 라이브러리

- spring-boot-starter-test
  - junit : 테스트 프레임워크 (4에서 5로 넘어가고 있는 추세)
  - spring-test : 스프링과 통합해서 테스트

## 3. view 환경설정

### resources

<img width="30%" src="https://user-images.githubusercontent.com/60567697/154985832-82eec992-f9c5-4ba3-aa91-a2b017ccd921.png"/>

> resources의 하위 폴더 static에는 메인 페이지의 html이 존재, templates에는 ViewName에 해당하는 html이 존재

### 동작 환경 (템플릿 엔진 이용)

<img width="70%" src="https://user-images.githubusercontent.com/60567697/154985123-08e0843a-e315-42e5-8fe8-0efafb6cd3a3.png"/>

> Controller에서 return 값으로 문자를 반환 -> viewResolver가 "resources:templates/ +{ViewName}+ .html"을 찾음

## 4. 빌드 & 실행 (윈도우)

1. cmd
2. gradlew build
3. cd build/libs
4. java -jar hello-spring-0.0.1-SNAPSHOT.jar
5. Ctrl + C -> 종료
- 실행 X -> 2번에서 gradlew clean build
- 인텔리제이의 포트 사용 종료

#회원 관리 예제

## 요구 사항
- 데이터: 회원ID, 이름
- 기능: 회원 등록, 조회
- 아직 데이터 저장소가 선정되지 않음(가상의 시나리오)

<img width="70%" src ="https://user-images.githubusercontent.com/60567697/158114699-f78d5882-8920-4d91-96ae-786af0998aeb.png"/>

- 컨트롤러: 웹 MVC의 컨트롤러 역할
- 서비스: 핵심 비즈니스 로직 구현
- 리포지토리: 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
- 도메인: 비즈니스 도메인 객체, 예) 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리됨

# 스프링 웹 개발 기초

## 1. 정적 컨텐츠

> 정적 컨텐츠는 서버에서 하는 일 없이 파일을 바로 웹브라우저에 내려준다.

 <img width="70%" src ="https://user-images.githubusercontent.com/60567697/155275377-f6f0625f-31e0-4392-bbc2-bd13691738ba.png"/>

### 다음 사진과 같이 hello-static.html을 바로 불러온다.

 <img width="40%" src ="https://user-images.githubusercontent.com/60567697/155275673-328d787c-3ef9-47e5-a684-1ba0fa4da59d.png"/>

## 2. MVC

> 관심사 분리가 필요 -> view, model, controller로 분리
>
> - view : 화면을 그림
> - model, controller : 로직 및 내부적인 처리

 <img width="70%" src ="https://user-images.githubusercontent.com/60567697/155276156-f8ace19e-2ad6-45ec-a25c-957f6eb29a23.png"/>

### html을 그대로 넘겨주지 않고 서버에서 프로그래밍해 동적으로 변경해서 전달한다.

## 3. API

> @ResponseBody를 사용해 http(헤더와 바디)의 바디부에 return 내용을 직접 넣어준다.

### - String 전달

#### view없이 넘겨받은 내용은 그대로 보여준다.

<img width="40%" src ="https://cdn.inflearn.com/public/files/posts/e9d3c5ef-6203-41c2-b442-34c9f11df529/blob"/><img width="40%" src ="https://cdn.inflearn.com/public/files/posts/e208d415-f85a-4efb-ba39-06b3de612b02/blob"/>

### - 객체 전달

#### json 방식으로 전달한다.

<img width="40%" src ="https://cdn.inflearn.com/public/files/posts/bd6f7fdb-b7d6-4b76-9433-d39111cb3172/blob"/>

### - @ResponseBody 사용원리

<img width="70%" src ="https://user-images.githubusercontent.com/60567697/155277252-7826ab4d-5009-48dc-92b4-91085529c826.png"/>

- HTTP의 BODY에 문자 내용을 직접 반환
- 컨트롤러에 @ResponseBody가 있으면 viewResolver에 넘기지 않고 그대로 데이터를 넘기기 위해 httpMessageConverter가 동작
- 문자처리 : StringConverter
- 객체처리 : MappingJackson2HttpMessageConverter
  - Jackson-> 객체를 json으로 변경하는 라이브러리

# 스프링 빈과 의존관계
> 스프링 빈   
> : Spring IoC 컨테이너가 관리하는 자바 객체  

## 1. 컴포넌트 스캔과 자동 의존관계 설정
- 회원 컨트롤러가 회원서비스와 회원 리포지토리를 사용할 수 있게 의존관계를 준비
1. 컴포넌트 스캔 방식
: 스프링이 생성될 때 컴포넌트 어노테이션이 있으면 스프링 빈으로 등록  
: @service, @repository에 @component 포함되어 있음

2. 자동 의존관계 설정  
: @Autowired 사용 -> 생성자에 @Autowired 가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣음  
: 이렇게 객체 의존관계를 외부에서 넣어주는 것이 의존성 주입 (DI,Dependency Injection)
  
<img width="70%" src ="https://user-images.githubusercontent.com/60567697/158117170-b417dc85-421f-45be-9c49-64a5ed1ad487.png"/>  

- 스프링 컨테이너에 스프링 빈을 등록할 때는 일반적으로 싱글톤으로 등록한다.

## 2. 자바 코드로 직접 스프링 빈 등록하기
- 회원 서비스와 회원 리포지토리의 @Service, @Repository, @Autowired 애노테이션을 제거하고
진행
- @Configuration을 단 클래그에 직접 빈을 등록한다. 여기서는 SpringConfig가 해당된다.

# 스프링 DB 접근 기술
## 1. H2 데이터베이스 사용
cmd에서 h2.bat을 이용해 실행  

## 2. Jdbc
<img width="70%" src ="https://user-images.githubusercontent.com/60567697/158128472-084bbeee-cbd2-4f40-bdb4-8f53117132b9.png"/>  

스프링의 DI (Dependencies Injection)을 사용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현
클래스를 변경 가능.

## 3. Jdbc Template
- 순수 Jdbc와 동일한 환경설정
- 스프링 JdbcTemplate과 MyBatis 같은 라이브러리는 JDBC API에서 본 반복 코드를 대부분
제거. 하지만 SQL은 직접 작성.  

## 4. JPA  
- JPA는 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행.
- JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임 전환 가능.
- JPA를 사용하면 개발 생산성 향상 가능.
- 
## 5. 스프링 데이터 JPA  
-> 추가 학습 필요

# AOP
## 1. AOP가 필요한 상황  
> AOP: Aspect Oriented Programming
- 메소드의 호출 시간을 측정하고자 할 때 사용
- 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern) 분리  

<img width="70%" src ="https://user-images.githubusercontent.com/60567697/158133793-57281532-1332-4a67-aaf1-d49d8804a9d0.png"/> 

## 2. AOP 동작방식
- AOP 적용 전  

<img width="70%" src ="https://user-images.githubusercontent.com/60567697/158134261-b2dcd251-36df-4224-a0f0-4c30bb228651.png"/>   

- AOP 적용 후  

<img width="70%" src ="https://user-images.githubusercontent.com/60567697/158134555-b89ee90e-5212-429c-aa4e-41fc9fea7a00.png"/>   
