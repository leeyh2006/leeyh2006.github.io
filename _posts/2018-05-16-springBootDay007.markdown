---
layout: post
title: SpringBoot day007 정리
date: 2018-05-16 00:00:00 +0300
description:  day 007 정리 # Add post description (optional)
img: # Add image post (optional)
tags: [SpringBoot] # add tag
---
> 백기선님 강의와 SpringBoot reference를 기반으로 정리하였습니다.

프로젝트리스트  
![Intellij LIst]({{site.baseurl}}/assets/img/spinrBootDay007intellijList.JPG)

###### 23.5 Application Events and Listeners
spring App이 동작되기 전에 events와 Listeners을 정의 할수 있다.

{% highlight ruby %}
MyLisener.java

public class MyListener implements ApplicationListener<ApplicationStartedEvent> {
    @Override
    public void onApplicationEvent(ApplicationStartedEvent applicationStartedEvent) {
        System.out.println("APPLICATION STARTED");
    }
}
{% endhighlight %}

ApplicationListenr 클래스에 이벤트 타입을 지정해주고
ex) *ApplicationStartingEvent*, *ApplicationEnvironmentPreparedEvent*,*ApplicationPreparedEvent* 등등 ..
 
 
 {% endhighlight %} 
    Application.java 
    
   /@EnableAutoConfiguration  spring boot에서 제공하는 annotation  
   //@Configuration
   //@ComponentScan  con 밑에  있는 클래스를 참조해서  bean으로 등록할 수 있는 건 등록함
   //    이걸로 @Configuration 을 픽업하게 만들어라
   //    
   
   @RestController
   @SpringBootApplication  @EnableAutoConfiguration , @Configuration 모아놓은것 
    public class Application {
        //주입 받고
        @Autowired
        private HelloService helloService;
        @RequestMapping("/")
        public String home(){
            return helloService.getMessage();
        }
        @Bean
        public ExitCodeGenerator exitCodeGenerator(){
            return () -> 42;
        }
        public static void main (String args [] ){
            SpringApplication app = new SpringApplication(Application.class);
            app.addListeners(new MyListener()); //*MyListenr() 이벤트나 리스너를 정의한것을 addListener 메소드를 통해 추가  *
            app.run(args);
       }
    }
{% endhighlight %}
    
#### 실행결과 창
![excuteResult]({{site.baseurl}}/assets/img/day007Result.JPG)  
