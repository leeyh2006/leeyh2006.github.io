---
layout: post
title: SpringBoot day19,20 정리
date: 2018-05-25 00:00:00 +0300
description:  SpringBoot day019,20 정리  # Add post description (optional)
img: springBootLogo.JPG # Add image post (optional)
tags: [SpringBoot] # add tag
---
### TEST 하기
테스텡 목적 ? - 의존성 주입의 장점으로서 내 코드를 유닛테스트에 더 쉽게 만들수 있다는 목적
###mockMvc 사용법


MOCK : WebApplicationContext 로딩함 . SeverletApI가 Classpath에 없으면 웹이 아닌 ApplicationContext를 만듬 
RANDOM_PORT : ServletWebServerApplicationContext 를 만들고 진짜 서블릿 환경을 제공 
DEFINED_PORT : ServletWebServerApplicationContext 를 만들고 진짜 서블릿 환경을 제공하고 define하는 port를 띄운다 
[pom.xml]
```xml 
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-test</artifactId>
     <scope>test</scope>
    </dependency>     
```
<scope>이  테스트이면 이 의존성은 test할때 밖에 사용 못함 .  

Detecting Test Configuration 스프링 부트에서 알아서 찾아줌 .

#### Test Configuration 제외시키기  
만약 Component scanning을 하고 있다면 , 

#### test running Server 동작하는 서버에서 테스팅 하기

