---
layout: post
title: SKT Tango-D 
date: 2019-04-04 00:00:00 +0300
description: SKT Tango 프로젝트 # Add post description (optional)
img: tango.jpg # Add image post (optional)
---

### SK TANGO-D(Metatron) 개발 및 운영 
### 프로젝트 기간 (2018.10 ~ 2019.04)
### 서비스 내용 
- SK텔레콤의 통신,T맵 등 대용량 데이터들을 hadoop cluster 에 올려 관리하고 가공하여 필요한 팀이나 혹은 타사에게 데이터를 제공하는 서비스 

### 담당 업무
#### ETL 개발  
개발 업무가 필요할 경우 데이터를 파싱하는 로직을 작성 하고 Java daemon을 배포.
- 전화 번호 암호화 logic 추가  

#### hadoop cluster management  
-  Grafana  
    - Grafana Web 모티너터링을 통해 hadoop cluster의 Name Node위 에서 돌아가고 있는 Job 들의 상태를 확인하고 , 메모리를 너무 많이쓰거나, 돌고 있는 mrJob이 NameNode 성능에 영향을 끼치는 경우 해당 Job을 Kill 또는 분석.  
    - 해당 job으로 인해 Pending 되는 현상 방지

#### Resource manager  
- node들을 monitoring 하고, Grafana webg을 통해 Vcore와 memory를 체크하여 pending 되는 현상을 추후에 방지.  

#### Hive 쿼리 작성   
- hive query로 etl shell sciprt를 작성하고 데이터 가공  
 
#### log 파일 관리  
- 서버 용량 관리 및 partition 생성 crontab 배치 작성    

### 기술 스택: Hadoop,java,linux,hive  

