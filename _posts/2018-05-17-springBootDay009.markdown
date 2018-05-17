---
layout: post
title: SpringBoot day009 정리
date: 2018-05-17 00:00:00 +0300
description:  day 009 정리 # Add post description (optional)
img: springBootLogo.JPG # Add image post (optional)
tags: [SpringBoot] # add tag
---

## YAML,Properties
>파일 확장자 .yml   
.properties 파일이 정형화 된 가운데 YAML 형식으로 property를 정의 하는 법에 대해서 정리하고자 함.

springBoot reference [Application Property Files](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-external-config-application-property-files) 에서 보다시피 기본적으로 applicadtion.properties 파일이나 applcaiton.yml 파일을 생성해주면 자동적으로 맵핑이된다.  
/config 경로나 다른 경로로 설정해서 지정할 수 있지만 직관적으로 그냥 설정 하는편이 낫다고 생각  
>YAML File  

![propertiesList]({{site.baseurl}}/assets/img/day009/YAMLfile.JPG)   
>application.properties   


```properties  
  yong.name=yong
  yong.yongList.descr = script
  yong.yongList.name = yong2
```

>application.yml  
properties의 key 값 아래로 .으로 접근 한다는 개념은 같음

```yaml  
application.yml
  ---
  key: 
    key1:  
      key2: value2 
      key3: value3
      key4: value4
  ---
  
  (list)
  key: 
      - key1 : value1
      - key2 : value2
```   
*---* 구분자로 하나의 yml 파일에 여러개 의 YAML 선언가능
키 값 밑에  *-* 을 통해서 list로 표현 가능   

>JAVA Bean 설정 방법    


1. @ConfigurationProperties(*Prefixr 값*) (Third-party Configuration 설정 방법)  
직접 application 클래스에서 빈으로 등록하여 사용  
```java  
   /* Application.class */
    @Autowired
    Environment environment;
   
    @SpringBootApplication
    @RestController
    public class Application{
    
        @Bean
        @ConfigurationProperties("key") // Bean으로 외부 YAML 파일을 주입받아 사용 
        public YongProperties yongProperties(){
                return new YongProperties(); 
        }
           
       @RequestMapping("/")
       public String home(){
            System.out.println(environment.getProperty("key.key1")); // 주입된 YAMl 파일을 통해 접근 
            System.out.println(environment.getProperty("key.key1.key2"));
            return "hello";
       }
        
       
       public static void main(String []args){
         SpringApplication app = new SpringApplication(Application.class);
         app.run(args);
       }
    }
    
    /* YongProperties.class */
     @Data
        public class YongProperties {
               private Map<Integer, String> key1 = new HashMap<>();
        }
```
2.그냥 @Component로 등록하여 사용 (외부파일이 아닌경우)  
```java  
    /* YongProperties.class */
    @Component
    @ConfigurationProperties("key") //key값을 주입 
    @Data
    public class YongProperties {
          private Map<Integer, String> key1 = new HashMap<>(); //key1을 통해서 접근 가능
    }
    
    /* Application.class */
    
     public class Application{
       
        @Autowired
        YongProperties yongProperties; //빈으로 등록된 yml 파일 사용
               
        @RequestMapping("/")
        public String home(){
             System.out.println(yongProperties.getKey2()); 
             
             return "hello"
         }    
        public static void main(String []args){
             SpringApplication app = new SpringApplication(Application.class);
             app.run(args);
           }
        } 
```

[24.7.5 @ConfigurationProperties Validation](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-external-config-validation)  
@ConfigurationProperties는 validation 기능도 설정 가능  

```java  
     @Data
     @Min(0) // 최소가 0
     @Max(10) //최대가 10
     //@NotEmpty ,@NotNull 등등 ..
     public class YongProperties {
              private Map<Integer, String> key1 = new HashMap<>(); //key1을 통해서 접근 가능
     }
```
>validation 체크  @NotNull

![ValidationError]({{site.baseurl}}/assets/img/day009/ValidationError.JPG)  
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

### 사용법
```java  
    @Data
    public class DAO {
            pirvate String name;
            private String age;
    }
```



외부에 있는 properties 파일에 직접적으로 @Comnponent 와 @ConfigurationProperties 어노테이션을 선언할 수 없는 경우 
위에 방식처럼 @Bean 으로 등록해서 사용 할 수 있다.
   
   
 24.7.2 
  '-' 가들어가 있는 문자열도 camelCase로 바인딩 해준다
  ex) yonghee.name-hello = yonghee (kebab case)

 [24.7.5 @ConfigurationProperties vs @Value](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-external-config-vs-value)  
 
### Profiles
 스프링 부트가 로드 되기 전에 제한을 설정 할수 있음 (ex 개발버전, 상용버전)
```java  
@Component
@Profile("dev") // dev 환경에서만 bean으로 등록하기
public class DevBean implements MyBean{
    @Override
    public String getMessage() {
        return "Dev Bean";
    }
}
```
--> 로드 되기전 profile이 *dev* 로 설정 되어 있기 때문에 로드 제한 걸림
```yaml  
spring.profiles:prod  //상용버전 으로 설정되어 있을경우 로드 제한

```
=