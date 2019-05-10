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
![cabbage1]({{site.baseurl}}/assets/img/cabbage1.jpg)    

### 담당업무
#### React와 node.js를 택한 이유 
1. Component의 재사용 

2. node.js
화상 채팅 기능 구현을 위해 

### 프로젝트 설계
다음은 3D 의안 Application 일부 화면이다. 각 메뉴의 페이지는 다음과 같은 페이지의 구성으로 공통적으로 사용한다.  
이를 공통화 하기위해 PageTemplate Component를 두고 그 안에 추가기능을 넣어 구현할 수 있도록 모듈을 만들었다.  

![cabbage2]({{site.baseurl}}/assets/img/cabbage2.jpg)   

PageTemplate.js 
```typescript jsx  
const PageTemplate=({children,name})=>(
    <div>
        <Header name={name}/>
        <div className="container">
            <SideMenu />
            <SearchBar/>
            <div className="contents_wrap">
                {children}
            </div>
        </div>
    </div>
);
```   

Page.js  
```typescript jsx     
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

