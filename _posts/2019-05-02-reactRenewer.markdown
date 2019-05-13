---
layout: post
title: 3D 의안 Application React 적용
date: 2019-04-09 00:00:00 +0300
description:  React renewer # Add post description (optional)
img: react.jpg # Add image post (optional)
---
### Stack : ReactJs, Node.js, mysql
### 프로젝트 기간 (2019.04 ~ 현재)
### 서비스 내용
병원에 내원하기 힘든 환자들에게 의안사와 화상채팅을 할 수 있는 환경을 조성하고 이를 통해  의안에 대한 데이터 모델을 정의 하고 제작 표안을 만드는 Application.
![cabbage1]({{site.baseurl}}/assets/img/cabbage1.jpg)    

### React 적용 이유
Spring 과 Jsp 로 구성되어 있는 기존 프로젝트를 앞단과 뒷단으로 분리하여 관리하고 싶은 욕심이 있었다. 
이를 위해 FE FrameWork를 모색하던중 다음과 같은 React의 장점이 끌렸다.

- JSX (js 안에 마크업 코드 사용)
- Component 재사용 
- 오직 View 만을 위한 Framework
- 빠른 Virtual Dom

위 장점을 토대로 기존 Spring Jsp 로 구성된 프로젝트와 비교해 봤을때, 기존 프로젝트에서 새로운 기능을 추가하려면
Jsp 파일과 그 와 관련된 js 파일을 모두 수정해야 되고 불 필요하게 중복적인 코드들이 작성되어야 한다는 단점이 있었다.  

코드 작성의 생산성도 높이고 오직 View 에만 집중하여 개발을 할 수 있다는 점이 제일 끌렸던 이유 인것 같다.

### 프로젝트 설계
다음은 3D 의안 Application 일부 화면이다. 화면의 기능들을 각각 Component화 하고 나누었다.
![cabbage2]({{site.baseurl}}/assets/img/cabbage2.jpg)   

#### 페이지 공통 모듈화   
각 메뉴의 페이지는 다음과 같은 페이지의 구성으로 공통적으로 사용한다.
PageTemplate Component를 두고 그 안에 추가기능을 넣어 구현할 수 있도록 모듈을 만들었다.    
![cabbage3]({{stie.baseurl}}/assets/img/cabbage3.jpg)

이렇게 공통 페이지 Component를 만들고 페이지를 렌더링 할때 각 페이지에서 컴포넌트를 불러와 사용.
![cabbage4]({{stie.baseurl}}/assets/img/cabbage4.jpg)


2. 데이터 상태관리를 위한 react-redux 적용

