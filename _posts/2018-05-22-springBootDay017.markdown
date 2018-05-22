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
    