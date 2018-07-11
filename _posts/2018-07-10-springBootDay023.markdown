---
layout: post
title: SpringBoot day23 정리
date: 2018-05-25 00:00:00 +0300
description:  SpringBoot day23 정리   # Add post description (optional)
img: springBootLogo.JPG # Add image post (optional)
tags: [SpringBoot] # add tag
---
###Reactive Programming 이란 ?
> 복수개의 서비스로 이루어진 분산 시스템이 정상 상황 뿐만 아니라 장애 상황에서도 일관된 동작을 보장해주는 시스템   
> asynchronous , non-blocking reactive 개발에 사용된다.  
> 서비스 간 호출이 많은 마이크로서비스 아키텍처에 적합
> servelet API 가 불 필요함 


### 개발 방식
> 기존의 @MVC 방식 (@Controller , @RestController, @RequestMapping)
> 새로운 함수형 모델 (RouterFunction , HandlerFunction)


### Example
maven 설정에서 `spring-boot-starter-web` 과 `spring-boot-start-webflux`가 동시에 존재할 시 스프링부트는 Spring MVC로 인식한다 .

> @MVC 방식
