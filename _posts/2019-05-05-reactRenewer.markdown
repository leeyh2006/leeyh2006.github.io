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
병원에 내원하기 힘든 환자들에게 의안사와 화상채팅을 할 수 있는 환경을 조성하고 이를 통해 의안을 제작하는데 도움을 주는 Application.
![cabbage1]({{site.baseurl}}/assets/img/cabbage1.jpg)    

#### React를 적용 하는 이유


2. node.js
화상 채팅 기능 구현을 위해 

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

