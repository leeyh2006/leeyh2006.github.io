---
layout: post
title: SpringBoot day017,18 정리
date: 2018-05-22 00:00:00 +0300
description:  SpringBoot day017,18 정리  # Add post description (optional)
img: springBootLogo.JPG # Add image post (optional)
tags: [SpringBoot] # add tag
---

[27.1.9 ConfigurableWebBindingInitializer](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-spring-mvc-web-binding-initializer)  
#### Converter 사용법

컨트롤러를 작성중에 원하는 값을 PathVariable로 받고싶을 때
예를 들어 현재 id의값을 Integer로 받고 있는데 이것을 BangSong 이라는 객체로 받아서 json으로 응답해주고 싶을 때   
Converter를 이용하여 데이터를 바인당 하는 방법이 총 3가지가 있다 .

```java  
    @RestController
    public class BangSongController {
        /* @GetMapping("/bs/{id}")
         public BangSong getBangsong(@PathVariable("id") Integer id){ //Integer가 아니라 Bangsong이라는 객체로 받고 싶다
            //binding이 안되고 스프링이 모르기 때문에 에러가 난다
            BangSong bangSong = new BangSong();
            bangSong.setId(id);
            return  bangSong;
        } */
        
        
        @GetMapping("/bs/{id}")
        public Bangsong getBangSong(@PathVariable BangSong bangsong)
        {
            return bangSong;
        }
    }
```
[BangSongConver.java]
```java  
    @Bean
    public class BangSongConverter implements Converter<String ,BangSong> {
        @Override
        public BangSong convert(String id) {
            BangSong bangSong=new BangSong();
            bangSong.setId(Integer.parseInt(id));
            return bangSong;
        }
    }
```
1. ConfigurableWebBindingInitializer 를 구현하여 컨버터 서비스를 등록해준다
    ```java  
      @Bean
        public ConfigurableWebBindingInitializer initializer(){
            ConfigurableWebBindingInitializer initializer =new ConfigurableWebBindingInitializer();
            ConfigurableConversionService conversionService = new FormattingConversionService();
            conversionService.addConverter(new BangSongConverter());
            initializer.setConversionService(conversionService);
            return initializer;
        }
        
    ```
    
2. WebMvcConfigurer를 상속받아 addFormatters를 오버라이드 하고 BangSongConverter를 등록해준다.  
    ```java  
        @Override
          public void addFormatters(FormatterRegistry registry) {
              registry.addConverter(new BangSongConverter());
          }
    ```
3. @Component 어노테이션을 이용하여 Converter를 Bean으로 등록해준다.
    ```java  
       @Component // 또하나의 방법 컨버터로 쓸 수 있는 그냥 Bean으로 등록
       public class BangSongConverter implements Converter<String ,BangSong> {
           @Override
           public BangSong convert(String id) {
               BangSong bangSong=new BangSong();
               bangSong.setId(Integer.parseInt(id));
               return bangSong;
           }
       }
    ```

Exception 사용법

@ControllerAdvice


Spring HATEOAS - 내가 지금 보내는 이 본문과 관련된 링크들을 같이 보내는 것 
 {
    "name" : "Alic",
    "links": [{
        "href":"http://localhost:8080/customer/1"
        
       ]
       
       
CORS란 ? 
원래 저희가 만들었던 ,컨트롤러들의 핸들러가 있을때 이러한것을 요청 할 수 있는애가 누구냐 같은 도메인에서 요청한 애들만 쓸수 있는데
cross Domain은 다른 도메인도 얘를 요청할 수 있도록 허용해 주는것
보통 API 를 그 API를 호출하는 쪽의 ORIGIN이 다르면 보통은 호출을 못한다 근데 그걸 우회 하는 방법은 Iframe으로 만들어서 하거나 jsonp 로 하는것

근데 그걸 공식적으로 지우너하는게 WC3에서 cors 

나의 Orgin :http://example.com 을 보내고

서버는 Access-Control-Allow-origin : http://example.com 나의 응답을 요청하는 도메인은 이러한 도메인이야 
모든 domain에서 요청하는 것을 다 받겠다  -> Access-Control-Allow-origin: *

부트에서는 @CrossOrigin 으로 사용
global cors 를 설정할 수 있다. (https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-cors)
