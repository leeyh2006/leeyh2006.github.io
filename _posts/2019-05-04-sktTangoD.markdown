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
- 데이터들이 저장되어 있는 서버로부터 ftp로 파일을 가져와 hadoop에 ETL을 통해 적재.
- Oozie 배치 스케쥴러 관리
- hadoop cluster Monitoring
- Data Node 용량 관리
- ETL 개발

#### Oozie 배치 스케쥴러 관리
일,주,월 단위로 각 시간에 맞춰 Mr Job이 수행 되어야 했는데 이를 Ooize를 통해 관리.  
주기에 맞춰 실행 되어야 하는 job 에 대해 cordinator ,subflow, workflow를 작성하고 oozie에 추가.

#### ETL 개발  
- 고객 정보 암호화   
데이터가 저장되어 있는 해당 서버로 부터 ftp로 파일을 읽어와 Line 별로 읽고 구분자를 통해서 Parsing 을 한 후 정규표현식을 적용하여 고객 정보를 `***-****-***` replace 한 후 다시 파일에 write.


#### hadoop cluster monitoring  
- Grafana Web 모티너터링을 통해 hadoop cluster의 Name Node위 에서 돌아가고 있는 Job 들의 상태를 확인하고 , 메모리를 너무 많이쓰거나, 돌고 있는 mrJob이 NameNode 성능에 영향을 끼치는 경우 해당 Job을 관리.  
- 이러한 job으로 인해 Pending 되는 현상 방지 

#### Data Node 용량 관리 
- log 파일 및 다른 서버로 부터 적재된 데이터들의 보관 주기를 정하고 이에 적합하지 않은 데이터들이나 log 파일을 지우는 Shell을 작성하고 crontab에 추가


### 기술 스택: Hadoop,java,linux,hive  

