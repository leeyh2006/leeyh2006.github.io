---
layout: post
title: 3D 의안 Application 아키텍처 전환
date: 2019-04-11 00:00:00 +0300
description:  React renewer # Add post description (optional)
img: react.jpg # Add image post (optional)
---
### 서비스 내용
병원에 내원하기 힘든 환자들에게 의안사와 화상채팅을 할 수 있는 환경을 조성하고 이를 통해 의안에 대한 데이터 모델을 정의 하고 제작 표안을 제공
![cabbage1]({{site.baseurl}}/assets/img/cabbage1.jpg)    

### 서비스 일부 화면
![cabbageMain]({{site.baseurl}}/assets/img/cabbageMain.jpg)    
--------------------------------------------------------------

### Back-end , Front-end 분리  
-   

### React 를 택하게 된 이유   

- JSX (js 안에 마크업 코드 사용)
- Component 재사용 
- 오직 View 만을 위한 Framework
- 빠른 Virtual Dom

Jsp 파일과 그 와 관련된 js 파일을 모두 수정해야 되고 불 필요하게 중복적인 코드들이 작성되어야 한다는 단점이 있었다.
코드 작성의 생산성도 높이고 오직 View 에만 집중하여 개발을 할 수 있다는 점이 제일 끌렸던 이유 인것 같다.

### Node.js 전환 계기


### 레거시 프로젝트 전환 과정


**아직 프로젝트는 진행중이고 ReactJs를 사내 FE 프레임워크에 적용하자고 어필을 하고 리딩중입니다.**

### 프로젝트 설계
--------------------------------------------------------------
### 화면 구성
다음은 3D 의안 Application 일부 화면이다.   
먼저 화면의 기능을 분리하기 위해 먼저 요소별로 Component화 할 대상을 설계 하였다.  

![cabbage2]({{site.baseurl}}/assets/img/cabbage2.jpg)   

### State 관리
#### Redux 도입 
- react-router , redux-pender 등 middleWare와 호환성이 좋음
- Redux dev tool , 액션의 흐름을 관찰하기 용이함
- 공유 되는 state를 store 한 곳에서 관리  

#### Ducks 구조 
redux를 사용하기 위해선 액션 타입,액션 생성 함수,리듀서 3가지 파일을 작성해야 했는데 이를 간편화 하기 위해 위 3가지를 한 파일에서 모듈화 시켰다. 
 
**module.js**   
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

    
### Route Path
아래와 같이 SideMenu에 각각의 Route Path 를 적용하는 도중 페이지를 이동 할때마다 Css가 적용이 안되는 현상이 있었다.
![cabbage6]({{site.baseurl}}/assets/img/cabbage6.jpg)  
#### 위 현상에 대한 해결점   
- store 에 페이지에 대한 상태값 저장
처음엔 당연히 페이지마다 각 페이지에대한 상태값을 가지고 있어야 된다고 생각하여 **store**에 페이지에 따른 상태값을 저장하고 이를 읽어와 분기를 태워 css를 적용하였다.  
하지만 이렇게 했을 경우 페이지가 늘어날 때마다 상태값 관리 포인트도 늘어나고 코드도 지저 분해지고 비효율적이라고 생각이 들었다.
- **NavLink 로 해결**
좀 더 효율적인 방법이 없을까 해서 검색해본 결과 Link 의 사용법은 알고 NavLink 사용법은 몰랐던 것이다.   NavLink를 사용하면 Path에 따른 스타일을 activeClassName 속성으로 관리 할수 있다는 것이었다.
state에 대한 관리 point도 적어지고 , 코드의 양도 줄어들어 좀 더 효율적이었다.  
```xml  
const SideMenu =() => (
    <div className='gnb_block'>
        <NavLink to="/video"  className='gnb_menu ico_cam' activeClassName="gnb_menu ico_cam selected" >
            <span className='gnb_txt'>화상상담</span>
        </NavLink>
        <NavLink to="/patient" className='gnb_menu ico_chart' activeClassName='gnb_menu ico_chart selected'>
            <span className='gnb_txt'>환자 정보</span>
        </NavLink>
        ..
        ..
        ..
              
    </div>
);
```

### 공통 기능 모듈화 작업
#### 1. Page
각 메뉴의 페이지는 다음과 같은 페이지의 구성으로 공통적으로 사용한다. 공통적으로 사용하지 않는 SearchBar 같은 경우 
PageTemplate Component를 두고 그 안에 추가기능을 넣어 구현할 수 있도록 모듈을 만들었다.    
  
**PageTemplate.js**  
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
공통 페이지 Template을 만들고 페이지를 렌더링 할때 각 페이지에서 컴포넌트를 불러와 재사용.  
**Page.js**  
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

#### 2. Button 
![cabbageButton]({{site.baseurl}}/assets/img/cabbageButton.jpg)    
반복적인 작업을 최소화 하기 위해 Button Component를 생성. 
```xml  
    const Div =({children,...rest})=><div {...rest}>{children}</div>
    
    const Button =({
                       children,to,onClick,disabled,theme='default',
                   })=>{
        const Element =(to && !disabled)? Link: Div;
        return(
                <Element
                    to={to}
                    className={theme}
                    onClick={disabled ? ()=>null:onClick}>
                    {children}
                </Element>
        )
    }
```


### 앞으로의 방향
아직 프로젝트가 진행 중이다. React를 사용하면서 컴포넌트별로 관리하다보니 기존 Spring3,Jsp 환경으로 구성되어있는 프로젝트보다 코딩이 좀 더 효율적으로 되었다.  
효율성을 극대화 하기 위해 아래 사항을 고려하여 프로젝트를 개발해 나갈 예정이다.
- 렌더링 최적화
- Chunk 파일을 통한 모듈 파일 관리
- 라우트 코드 스플리팅 
- SSR  
