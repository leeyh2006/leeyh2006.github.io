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

