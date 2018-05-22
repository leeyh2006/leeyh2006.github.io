---
layout: post
title: SpringBoot day015,016 정리
date: 2018-05-22 00:00:00 +0300
description:  SpringBoot day015,016 정리  # Add post description (optional)
img: springBootLogo.JPG # Add image post (optional)
tags: [SpringBoot] # add tag
---

[27.1.10 Template Engines](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-spring-mvc-template-engines)  

spring boot는 suffix 기능을 껐다 ex) "GET /projects/spring-boot.json"   
이런 매칭이오면 @GetMapping("/projects/spring-boot") 에 맵핑이 안된다 .

##### template Engines 설정 하기
여러가지 기호에 맞게 템플릿을 설정 할 수 있는데 그 중 FreeMarker 를 사용해서 설정해 보겠다.
기본적으로 스프링 부트에서는 resources/templates/** 를 정적 리소스로 제공한다.
나는 templates/hello.ftl을 생성해 주었다.  
![ftlPath]({{site.baseurl}}/assets/img/day015/ftlPath.JPG)
1. pom.xml 에  freemarker depenecy 추가     
    [pom.xml]
    ```xml    
       <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-freemarker</artifactId>
       </dependency>
    ```
2. Controller 생성
    ```java  
        @Controller
        public Class WebController{
            @GetMapping("/hello")
            public String hello(Model model, @RequestParam(defaultValue="Yong" String name)
            {
                model.addAttribute("name", name);
                return "hello";
            }
        }
    ```
3. ftl 생성
    ```ftl  
        <html>
        <head>
            <title>Welcome!</title>
        </head>
        <body>
        <h1>Welcome ${name}!</h1>
        </body>
        </html>
    ```  
    ![WelcomYong]({{site.baseurl}}/assets/img/day015/fltWelcome.JPG)
이와 같이 원하는 템플릿을 pom.xml 에 dependency로 추가하여 사용할 수 있다. (Groovy,Thymeleaf,Mustache .. )
---------

##### Error handling 참조
[27.1.11 Error Handling](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-error-handling)

##### Custom Error Page 설정 
1. spring boot reference에 친절하게 설명이 나와있다.
*/error* path를 지원하는데 기본적으로 커스텀 에러 페이지가 없으면 스프링 부트에 내장되어 있는 whiteLabel error 페이지를 표시한다.  
이러한 설정을 바꾸고 싶으면 정적 리소스 경로에 error 페이지를 추가해 주면 된다.  
![errorPath]({{site.baseurl}}/assets/img/day015/errorPath.JPG)  
![public.error.path]({{site.baseurl}}/assets/img/day015/staticErrorPath.JPG)
  [404.html]  
    ```html  
          <!DOCTYPE html>
          <html>
          <body>
            <h1>Something went wrong! 404 error </h1>
            <h2>Our Engineers are on it</h2>
            <a href="/">Go Home</a>
          </body>
          </html>  
    ```
    [500.html]
     ```html  
        <!DOCTYPE html>
        <html>
        <body>
            <h1>Something went wrong! 500 error </h1>
            <h2>Our Engineers are on it</h2>
            <a href="/">Go Home</a>
        </body>
        </html>
     ```
     
     ![WebError]({{site.baseurl}}/assets/img/day015/webError.JPG)
---- 
ERROR 페이지 설정 끝!



 
