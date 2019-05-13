---
layout: post
title: 오픈클립 PC버전 
date: 2019-05-05 00:00:00 +0300
description: openclib Project# Add post description (optional)
img: openclib.jpg # Add image post (optional)
---
### Stack : nodeJs,angularJs,electron
### 프로젝트 기간 (2018.07 ~ 2018.10)
### 서비스 내용
- 사내 전사 자료 관리 및 전자 결재 서비스 데스크톱 앱 버전  
![openclib1]({{site.baseurl}}/assets/img/openclib1.jpg)    

### 담당 업무    
기존 사내 SNS Application은 web 서비스로 되어 있었고 이를 데스크톱 앱으로 새롭게 리뉴얼 하는 업무.    
1. 기존 SNS 채팅 기능 개발
2. 화상채팅 기능 추가  

### 프로젝트 설계
### Framework 선정
#### node.js  
javascript 기반으로 개발 구조가 매우 단순 했던 점과, 현 프로젝트에 주 목적은 채팅기능과 화상채팅 기능 이였는데 이를 socket.io 모듈을 통해 쉽게 기능을 구현할 수 있다는 점에 택 하게 되었다.
또한 데스크톱 앱 배포를 위해서 조사를 하던 중 Electron 을 접하게 되었는데 이 또한 node.js를 사용하여 application을 제작할 수 있는 이점이 있었다.
#### angularJs

#### 화상채팅 구현
다음은 openclib-pc 버전 화상채팅 기능 일부 화면이다.
![openclib3]({{site.baseurl}}/assets/img/openclib3.jpg)


#### WebRtc 서버



#### 마치며
