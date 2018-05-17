---
layout: post
title: SpringBoot day009 정리
date: {{ site.time | date: '%y' }}
description:  day 009 정리 # Add post description (optional)
img: springBootLogo.JPG # Add image post (optional)
tags: [SpringBoot] # add tag
---
[TOC]
## YAML 작성법
spring.user.password default에서만 사용 가능
negeated 제외된


## LOMBOK 이란 ?
```java  
   Test.java
   public class DAO {
        pirvate String name;
        private String age;
        ...
        ...
        public void setName(String name){
            this.name =name;
            }
        public String getName(){
            return this.name;
            }
         ...
    }   
```  
setter와 getter를 일일히 생성하고 변경해줘야 하는 번거로움을 해결해주는 자동 생성 라이브러리  

###사용법
```java  
    @Data
    public class DAO {
            pirvate String name;
            private String age;
    }
```
