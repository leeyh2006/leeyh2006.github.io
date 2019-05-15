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
- SK텔레콤의 통신,T맵 등 대용량 데이터들을 hadoop cluster 에 올려 관리하고 가공하여 필요한 팀에게 데이터를 제공하는 서비스 

### 담당 업무

#### hadoop cluster 관리
- hadoop cluster 로 구성된 data node 들의 상태를 관리하고 수집된 데이터를  etl을 통해 가공하고 제공

#### monitoring

#### log 파일 관리

- resource manager를 통해 node들을 monitoring 하고, Grafana webg을 통해 Vcore와 memory를 체크하여 pending 되는 현상을 추후에 방지.
- 개발 업무가 필요할 경우 데이터를 파싱하는 로직을 작성 하고 Java daemon을 배포 
- hive query로 etl shell sciprt를 작성하고 데이터 가공
- 서버 용량 관리 및 partition 생성 crontab 배치 작성  


### 기술 스택: Hadoop,java,linux,hive 

