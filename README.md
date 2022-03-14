# 목차

---

- #### [Spring Boot 환경 설정](#spring-boot-환경-설정)
- #### [스프링 웹 개발 기초](#스프링-웹-개발-기초)

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

<img width="60%" src="https://user-images.githubusercontent.com/60567697/154985123-08e0843a-e315-42e5-8fe8-0efafb6cd3a3.png"/>

> Controller에서 return 값으로 문자를 반환 -> viewResolver가 "resources:templates/ +{ViewName}+ .html"을 찾음

## 4. 빌드 & 실행 (윈도우)

1. cmd
2. gradlew build
3. cd build/libs
4. java -jar hello-spring-0.0.1-SNAPSHOT.jar
5. Ctrl + C -> 종료
- 실행 X -> 2번에서 gradlew clean build
- 인텔리제이의 포트 사용 종료

# 스프링 웹 개발 기초

## 1. 정적 컨텐츠

> 정적 컨텐츠는 서버에서 하는 일 없이 파일을 바로 웹브라우저에 내려준다.

 <img width="60%" src ="https://user-images.githubusercontent.com/60567697/155275377-f6f0625f-31e0-4392-bbc2-bd13691738ba.png"/>

### 다음 사진과 같이 hello-static.html을 바로 불러온다.

 <img width="40%" src ="https://user-images.githubusercontent.com/60567697/155275673-328d787c-3ef9-47e5-a684-1ba0fa4da59d.png"/>

## 2. MVC

> 관심사 분리가 필요 -> view, model, controller로 분리
>
> - view : 화면을 그림
> - model, controller : 로직 및 내부적인 처리

 <img width="60%" src ="https://user-images.githubusercontent.com/60567697/155276156-f8ace19e-2ad6-45ec-a25c-957f6eb29a23.png"/>

### html을 그대로 넘겨주지 않고 서버에서 프로그래밍해 동적으로 변경해서 전달한다.

## 3. API

> @ResponseBody를 사용해 http(헤더와 바디)의 바디부에 return 내용을 직접 넣어준다.

### - String 전달

#### view없이 넘겨받은 내용은 그래도 보여준다.

<img width="40%" src ="https://cdn.inflearn.com/public/files/posts/e9d3c5ef-6203-41c2-b442-34c9f11df529/blob"/><img width="40%" src ="https://cdn.inflearn.com/public/files/posts/e208d415-f85a-4efb-ba39-06b3de612b02/blob"/>

### - 객체 전달

#### json 방식으로 전달한다.

<img width="40%" src ="https://cdn.inflearn.com/public/files/posts/bd6f7fdb-b7d6-4b76-9433-d39111cb3172/blob"/>

### - @ResponseBody 사용원리

<img width="60%" src ="https://user-images.githubusercontent.com/60567697/155277252-7826ab4d-5009-48dc-92b4-91085529c826.png"/>

- HTTP의 BODY에 문자 내용을 직접 반환
- 컨트롤러에 @ResponseBody가 있으면 viewResolver에 넘기지 않고 그대로 데이터를 넘기기 위해 httpMessageConverter가 동작
- 문자처리 : StringConverter
- 객체처리 : MappingJackson2HttpMessageConverter
  - Jackson-> 객체를 json으로 변경하는 라이브러리
