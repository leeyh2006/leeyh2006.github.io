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

### Back-end , Front-end 분리
- 앞단과 뒷단의 관리포인트를 줄이고 SPOF를 없애기 위해 분리

### 레거시 프로젝트 전환 과정

#### Back-end config 설정 변경
API Server로 만 사용될 목적으로 변경하기 위해 아래와 같은 작업 수행.
1. **jsp 걷어내기**
- dispatcher.xml 내 JSP가 맵핑되있는 path 및 InternalResourceViewResolver 수정
- Controller 내 Jsp View return 기능 삭제
2. **Web static resource 를 FE server 로 이관**  


#### React (FE) 선택   
- JSX (js 안에 마크업 코드 사용)
- Component 재사용 
- 오직 View 만을 위한 Framework
- 빠른 Virtual Dom  

Spring3, jsp 구조에선 Jsp 파일과 그 와 관련된 js 파일을 모두 수정해야 되고 불 필요하게 중복적인 코드들이 작성되어야 한다는 단점이 있었다. 이에 반해 React 의 컴포넌트형 개발을 함으로써 효율성을 극대화 시킨점이 제일 끌렸다.  

**아직 프로젝트는 진행중이고 ReactJs를 사내 FE 프레임워크에 적용하자고 어필을 하고 리딩중입니다.**  

## 프로젝트 설계
### 1. React (FE)
#### 1.1 Component 디자인 설계 
다음은 서비스 일부 화면이다. 이를 컴포넌트화 하기 위해 각각의 기능을 다음과 같은 기준으로 분리하였다.  
1. presentational component , container component 기준으로 분리 
2. 각 페이지의 제일 큰 단위 -> 작은 단위 별로 분리  
![pgaeSplit]({{site.baseurl}}/assets/img/pgaeSplit.jpg)     

### 1.2 State 관리
#### Redux 적용 이유    
- react-router , redux-pender 등 middleware와 호환성이 좋음
- Redux dev tool , 액션의 흐름을 관찰하기 용이함
- 공유 되는 state를 store 한 곳에서 관리  
 
### 1.3 공통 기능 모듈화 작업
#### 1.3.1 Page
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

#### 1.3.2 Button 
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


--------------------------------------------------------------  

### 전환 작업중 잘못 접근 했던 점.
#### 1. 페이지 전환에 따른 css 미적용
아래와 같이 SideMenu에 각각의 Route Path 를 적용하는 도중 페이지를 이동 할때마다 css가 적용이 안되는 현상이 있었다.
![cabbage6]({{site.baseurl}}/assets/img/cabbage6.jpg)  
  
#### 위 상황에 대한 해결 접근 방법 
1. redux-store 에 각 페이지 정보 저장  
Store를 통해 PageName 정보를 받아와서 파싱    

```xml     

class SideMenu extends Component{
    render() {
        const {pageName} = this.props;
        return(
            <div className='gnb_block'>
                {
                    pageName ==='viedo' ?
                        <Link to="/video" className='gnb_menu ico_cam' >
                            <span className='gnb_txt'>화상상담</span>
                        </Link>
                        :
                        <Link to="/video"  className='gnb_menu ico_cam selected'>
                            <span className='gnb_txt'>화상상담</span>
                        </Link>
                }
                ...
                ...
                
            </div>
        )
    }
}

export default connect(
    (state,action) =>{
        currentPage: state.current('pageName');
    },
    null
)(SideMenu);
```  
위와 같은 방법으로 코드를 작성 했을 경우 코드양도 늘어나고, 파싱 과정 중 중복적으로 Link를 사용해야됬다. 비효율적이라 생각이 들어 **다른방법을 모색**    
- **NavLink** 로 해결  
NavLink를 사용하면 Path에 따른 스타일을 activeClassName 속성으로 적용 할수 있는 점이 있었다.  

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
코드의 가독성도 올라갔고, Store 에 state 관리포인트에 신경 쓸 필요가 없어 더욱더 효율적이였다.

### 앞으로의 방향  
React를 사용하면서 컴포넌트별로 관리하다보니 기존 jsp 보다 중복으로 코드를 작성하는 경우는 줄어 들었고, input type="hidden" 을 사용하여 data의 상태를 관리했던 점을 redux 를 사용하여 관리하다 보니 좀더 가시성이 뛰어났다.
아직 프로젝트는 진행중이고, 아래 다음 사항을 고려하여 개발을 진행해 나갈 예정이다.
- 렌더링 최적화
- Chunk 파일을 통한 모듈 파일 관리
- 라우트 코드 스플리팅 
- SSR 고려
