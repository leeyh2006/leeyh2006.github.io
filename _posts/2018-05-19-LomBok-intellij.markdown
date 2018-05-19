layout: post
title: intellij lombok 설정
date: 2018-05-19 00:00:00 +0300
description:  intellij lombok 설정 정리 # Add post description (optional)
img: # Add image post (optional)
tags: [intellij Setting] # add tag
---
### intellij lombok 설정
#### lombok plugin 설치  
ctrl +shift +s -> plugin -> lombok plugin -> install ->restart 
![lombokPlugin]({{site.baseurl}}/assets/img/lombok/lombokSetting.JPG)     

![lombokPluginRestart]({{site.baseurl}}/assets/img/lombok/lombokPlugin.JPG)  

#### lombok 설정

ctrl + shift+ s -> Build, Execution ,DevelopMent -> complier -> Annotation Processor
EnableAnnotaion process (체크)

![settingLombokintellij]({{site.baseurl}}/asstes/img/lombok/settingLombokintellij.JPG)

#### pom.xml dependency 추가
[Maven Repository lomBok depency](https://mvnrepository.com/artifact/org.projectlombok/lombok)
위 경로에서 dependency 복사후 pom.xml 에 추가
```xml  
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.16.20</version>
            <scope>provided</scope>
        </dependency>
```

설정 끝 !