---
layout: post
title: 3D 의안 Application React 적용
date: 2019-05-05 00:00:00 +0300
description:  React renewer # Add post description (optional)
img: react.jpg # Add image post (optional)
---
### Stack : ReactJs, Node.js, mysql
### 프로젝트 기간 (2019.04 ~ 현재)
### 서비스 내용
병원에 내원하기 힘든 환자들에게 의안사와 화상채팅을 할 수 있는 환경을 조성하고   
이를 통해  의안에 대한 데이터 모델을 정의 하고 제작 표안을 만드는 Application.
![cabbage1]({{site.baseurl}}/assets/img/cabbage1.jpg)    

#### React 적용 이유
Spring3 와 Jsp 로 구성되어 있는 기존 프로젝트를 앞단과 뒷단으로 분리하여 관리하고 싶은 욕심이 있었다.   
이를 위해 FE FrameWork를 모색하던중 다음과 같은 React의 장점이 끌렸다.
- JSX 
- Component의 재사용
- 오직 View 만을 위한 Framework


### 프로젝트 설계
다음은 3D 의안 Application 일부 화면이다. 각 메뉴의 페이지는 다음과 같은 페이지의 구성으로 공통적으로 사용한다.  
이를 공통화 하기위해 PageTemplate Component를 두고 그 안에 추가기능을 넣어 구현할 수 있도록 모듈을 만들었다.  

![cabbage2]({{site.baseurl}}/assets/img/cabbage2.jpg)   

```javascript       
const PatientPage =()=>{
    return(
        <PageTemplate name="환자정보">
            <ListTemplate
                tblHeader={<PatientHeaderContainer/>}
                tblBody={<PatientListContainer/>}
                tblFooter={<PatientFooterContainer/>}
            />
        </PageTemplate>
    )
}
```

2. 데이터 상태관리를 위한 react-redux 적용

