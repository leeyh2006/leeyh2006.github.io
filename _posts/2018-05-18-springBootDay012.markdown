---
layout: post
title: SpringBoot day012,013,014 정리
date: 2018-05-17 00:00:00 +0300
description:  day012,013,014  정리 # Add post description (optional)
img: springBootLogo.JPG # Add image post (optional)
tags: [SpringBoot] # add tag
---

[27.1.1 Spring MVC Auto-configuration](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-spring-mvc-auto-configuration)   
스프링 *default* 설정은 다음과 같다 
 1. ContentNegotiatingViewResolver 와  BeanNameViewResolver 포함.  
    - ContentNegotiatingViewResolver ?  요청 url MediaType에 따라 응답결과를 다르게 처리할수 있도록 도와주는 역할
    - BeannameViewResolver view의 이름을 찾을 때 bean의 이름으로 찾게 해주는 역할 
       ```java  
         /**
             * "main" -> ViewResolver*-> View*라는 객체로 바꿔야됨 -> 뭘 내보낼꺼냐 -> ContentNegotiatingViewResolver ->  View
             * @return
             */
       @RequestMapping
           public String index(){
               return "main"; //메인이라는 view이름으로된 bean을 찾아서 view를 Bean이 제공하는 컨텐츠로 제공 
           }  
       ```
 2. 정적 리소스에 webJar파일 지원 
    [webJars.org](https://www.webjars.org/)  
    - webJars란 ?   
webjar(프론트엔드 라이브러리를 jar파일형식으로 패키징 해놓은것)형식으로 패키지가 되어있으면 그 jarfile 안에서 그 리소스를 제공  
   ex) 나는 maven 으로 설정 했기 때문에 다음과 같은 dependency 를 추가할 수 있다.  
    ![webJarsMaven]({{site.baseurl}}/assets/img/day012/webJars.JPG)   
    *pom.xml*
    ```xml  
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.3.1-1</version>
        </dependency>
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>webjars-locator-core</artifactId>
        </dependency>
    ```
    *Maven Library에 추가된 것을 확인*   
    ![webJarsPath]({{site.baseurl}}/assets/img/day012/webJarsPath.JPG)   
    다음과 같은 dependency를 추가하여, jquery 라이브러리를 사용 할 수 있게 되었다. 이를 template에서 사용하는 법을 알아보자  
    이 리소스들은 스프링 부트 레퍼런스에 나와 있듯이 /webjars/** 에 위치한다.   
    버전을 명시하여 쓰는 방법이 있는데 ,아래와 같이 사용 하려면 webjars-locator-core dependency를 추가해줘야 한다.
     
    ```html 
       <script type="text/javascript" src="webjars/jquery/jquery.min.js"/>
    ```
 3. Converter, GenericConverter, Formatter bean 자동 설정
 4. HttpMessageConverters 지원
     ```java
        /**
         * 요청에 json 이 들어온다(webJars)
             * {"name":"yong"}
             * public Yong index(Yong yong){
             *     return yong; ->어떻게 json으로 바꿔줄 것이냐
             * }
         */
     ```
 5. MessageCodesResolve 자동 등록
 6. 정적 index.html 지원
    - static Content
      -spring mvc에서는 기본적으로 /static 에서 핸들링 하고 있는데
      webMvcConfiguer에서 addResourceHandlers에서 핸들링해서 써라
      
      static resource를 기본적으로 resource 폴더 밑에 static directory를 생성해주면 
      spring boot는 자동적으로 static 리소들이 맵핑이된다
      그런데 static 리소스들을 localhost:8080/경로 형식으로 말고 localhost:8080/static/경로 로 사용하고싶으면
       
      application.properties 파일에서 spring.mvc.static-path-pattern=/static/** 를 추가해서 사용하면된다   
      
      */resources/static/index.html*  
       ![indexPath]({{site.baseurl}}/assets/img/day012/indexP.JPG)   
 7. favicon 지원
 8. ConfigurableWebBindingInitializer bean 자동 사용
 
 
--------
위에 기능들을 유지하고 추가 설정 방법

```java  
  @Configuration 사용 
  public class WebConfig implements WebMvcConfigrue{
        /**
        *  WebMvcConfigure의 기능들을  ovride 해서 설정 하고 사용 
        
        */ 
  }
```

jackson을 사용해서 serialize and deserialize 를 할 수 있는데
스프링 부트는 @JsonComponent를 제공함
.

->jar패키징 할때는 사용하지 마세요 

(pom.xml)버전에 상관없이 로케이트를 주려면 webjars-locator-core 를 추가해주면된다
spring boot는 suffix 기능을 껐다 ex) "GET /projects/spring-boot.json"   
이런 매칭이오면 @GetMapping("/projects/spring-boot") 에 맵핑이 안된다 .

![spring project Initializer](https://start.spring.io/)