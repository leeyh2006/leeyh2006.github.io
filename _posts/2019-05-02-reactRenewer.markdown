---
layout: post
title: 3D 의안 Application React 적용
date: 2019-04-09 00:00:00 +0300
description:  React renewer # Add post description (optional)
img: react.jpg # Add image post (optional)
---
### Stack : ReactJs, Node.js, mysql
### 프로젝트 기간 (2019.04 ~ 진행중)
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

위 장점을 토대로 기존 **Spring Jsp 로 구성된 프로젝트**와 비교해 봤을때, 기존 프로젝트에서 새로운 기능을 추가하려면
Jsp 파일과 그 와 관련된 js 파일을 **모두 수정**해야 되고 불 필요하게 중복적인 코드들이 작성되어야 한다는 단점이 있었다.  
코드 작성의 생산성도 높이고 오직 View 에만 집중하여 개발을 할 수 있다는 점이 제일 끌렸던 이유 인것 같다.

**아직 프로젝트는 진행중이고 ReactJs를 사내 FE 프레임워크에 적용하자고 어필을 하여 개인적으로 진행하였다.**
### 프로젝트 설계

### state 관리
- Component Depth 로 인한 상태 전달의 비효율성  
    - 프로젝트 진행중 Component 를 재사용 하면서 상태값을 넘겨 줄때 depth 가 깊은 Component들 간의 상태 값 전달은 비효율 적이고 코드의 효율성도 떨어졌다. 이를 좀더 효율적으로 state 를 관리 할 수 있는 방법에 대해 모색하던 중 **react-redux** 와 **ContextAPI** 모듈을 찾았다.  
- react-redux ? ContextAPI ?  

#### Duck Structure
redux를 사용하기 위해선 액션 타입,액션 생성 함수,리듀서 3가지 파일을 작성해야 했는데 이를 간편화 하기 위해 위 3가지를 한 파일에서 모듈화 시켰다.  
ex) **module.js**  
```javascript    
    import {createAction,handleActions} from 'redux-actions';
    import * as api from '../../lib/api';
    ..
    ..
    //action Types
    const GET_PATIENT = 'patient/GET_PATIENT';
    //action creators
    ..
    export const getPatient = createAction(GET_PATIENT, api.getPatient);
    ..
    //reducer
    export default handleActions({
        ..
        
        ...pender({
            type:GET_PATIENT,
            onSuccess:(state,action)=>{
                const{ data } = action.payload;
                return state.set('patient',fromJS(data));
            }
        })
        ..
        
    },initialState);
    
```   

#### 최종 구조 
![cabbage5]({{stie.baseurl}}/assets/img/cabbage5.jpg)    

다음은 3D 의안 Application 일부 화면이다. 화면의 기능들을 각각 Component화 하고 나누었다.
![cabbage2]({{site.baseurl}}/assets/img/cabbage2.jpg)   

#### 페이지 공통 모듈화   
각 메뉴의 페이지는 다음과 같은 페이지의 구성으로 공통적으로 사용한다.
PageTemplate Component를 두고 그 안에 추가기능을 넣어 구현할 수 있도록 모듈을 만들었다.    
```xml      
const PageTemplate = ({ children ,match}) => (
    <div>
        <SideMenu />
        <div className="container">
            {
                match.path ==='/video/' ? ''  :  <SearchBar/>
            }
            <div className="contents_wrap">
                {children}
            </div>
        </div>
    </div>
);


```

### 화면 구성
이렇게 공통 페이지 Component를 만들고 페이지를 렌더링 할때 각 페이지에서 컴포넌트를 불러와 재사용.  
ex) **Page.js**  
```xml      
const PatientPage =()=>{
    return(
            <PageTemplate >
                <ListTemplate
                    tblHeader={<PatientHeaderContainer/>}
                    tblBody={<PatientListContainer/>}
                    tblFooter={<PatientFooterContainer/>}
                />
            </PageTemplate>
    )
}

```





