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

BookController.java

```java  
    /**
     * Flux- 0개에서 N개의 item을 가지고 있는 퍼블리셔
     */
    @GetMapping("/flux")
    public Flux<String> flux(){
        return Flux.just("a","b","c");
    }

    /**
     * 똑같은 퍼블리셔인데 flux에 각각들어있는 item에 대해서 String으로 나가는거
     * Mono 는 0개에서 1개
     */
    @GetMapping("/mono")
    public Mono<String> mono(){
        return Mono.just("mono");
    }
```

> RouterFunction 방식     

BookHandler.java  
- 여러개의 값을 리턴하고 싶은 경우 FLux.just로 설정 
```java  
//이거를 bean 에다가 넣을수 있음 Application class 참조
@Controller
public class BookHandler {
    public Mono<ServerResponse> allbooks(ServerRequest request){ // Mono로 설정해야되고 ServerResponse로 설정  ServerRequest로 파라미터 받고
        Book book1 = new Book();
        book1.setIsbn("1123");
        book1.setTitle("Book Spring Boot");
        Mono<Book> mono = Mono.just(book1);

        Book book2 = new Book();
        book2.setIsbn("1123");
        book2.setTitle("Book Spring Boot");

        /**
         * Book이 여러개 일경우 return 해줘야 할 값이 여러개
         */
        Flux<Book> books = Flux.just(book1,book2);


        return ServerResponse.ok().contentType(MediaType.APPLICATION_JSON).body(books, Book.class);
    }
}

```
Application.java

```java  

@SpringBootApplication
public class Application {


    @Autowired
    BookHandler bookHandler; // bookHandler를 주입 받아서 

    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    }

    @Bean  //RouterFunction을 Bean으로 설정하여 Handler의 기능 사용 
    public RouterFunction<ServerResponse> rount(){
        return route(GET("/testFlux").and(accept(MediaType.APPLICATION_JSON)),bookHandler::allbooks);
    }
}

```
