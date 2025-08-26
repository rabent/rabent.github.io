---
title: PORTFOLIO
permalink: /resume/
layout: page
excerpt: 
comments: false
---

![emoji](/assets/img/1f468-1f4bb.png){: width="50" height="50"}
# **이도준 | Backend Programmer**


## Contact & Blog
---
+ Email \| rabent0207@gmail.com
+ Blog \| rabent.github.io

## 목차
---
* [경력](#경력)  
* [기술 스택](#기술-스택)  
* [프로젝트](#프로젝트-경험)  
* [활동 내용](#활동-내용)  


## 경력
---
+ 홍익대학교 컴퓨터공학과 재학  (2017~)
+ 대학교 내 게임 제작 동아리 EXP에서 서브프로그래머로 프로젝트 참여
+ 대학교 졸업 프로젝트 '젬스톤 서바이버'의 기획, 메인프로그래머로 활동  
+ SSAFY 입과(2025~)  

## 기술 스택
---
- Unity Engine
- C++
- C#  
- Mysql  
- JAVA  
- Spring  
- Docker  
- Jenkins  


## 프로젝트 경험
---
Oh-Mok!
- Photon 서버를 이용한 1대1 오목 보드게임
- 구글 플레이스토어 출시
- 클라이언트 : Unity Engine
- 서버 : Photon
- 개발 기간 : 8개월
- 관련 링크 :   
[[블로그 내 포스팅]](https://rabent.github.io/Oh-Mok!/)  
[[Github 링크]](https://github.com/nilbace/Oh-MOK)  
[[Play store 링크]](https://play.google.com/store/apps/details?id=com.ExPStudio.magical)

젬스톤 서바이버  
- 정통 핵앤슬래시의 시스템을 접목한 로그라이트 핵앤슬래시 게임
- 홍익대학교 졸업프로젝트
- 클라이언트 : Unity Engine  
- 개발 기간 : 10개월  
- 관련 링크 :  
[[블로그 내 포스팅]](https://rabent.github.io/%EC%A0%AC%EC%8A%A4%ED%86%A4-%EC%84%9C%EB%B0%94%EC%9D%B4%EB%B2%84-%EB%A6%AC%EB%B7%B0/)  
[[Github 링크]](https://github.com/rabent/gemstone)   

Hamgaja!  
- 관광지를 검색하고 여행 계획을 수립할 수 있는 여행 정보 플랫폼  
- Spring, Vue를 기반으로 Docker, nginx 등을 사용하여 제작  
- 실무에서 자주 사용되는 툴을 실제로 사용해보기 위한 프로젝트  
- 개발 기간 : 2개월  
- 관련 링크 :  
[[블로그 내 포스팅]](https://rabent.github.io/%EA%B4%80%ED%86%B5-%EC%A0%95%EB%A6%AC/)  
[[Github 링크]](https://github.com/rabent/Hamgaja)  

Matching-SSAFY  
- 부트캠프 내 원할한 팀 빌딩을 돕고 관리자가 통계를 한 눈에 볼 수 있도록 하는 플랫폼  
- 기존의 Github action을 통한 CI/CD에서 더욱 확장된 Jenkins를 통한 CI/CD로 발전  
- 개발 기간 : 2개월  
- 관련 링크 :  
[[블로그 내 포스팅-회고]](https://rabent.github.io/%EA%B3%B5%ED%86%B5-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%ED%9A%8C%EA%B3%A0/)  
[[블로그 내 포스팅-트러블슈팅]](https://rabent.github.io/%EA%B3%A0%EB%AF%BC%EB%93%A4/)  


## 활동 내용
---  

### 성능 개선  

**대쉬보드 기능 성능 개선**  
&nbsp;  
+ 문제 : 대쉬보드 기능은 통계를 내야 하기 때문에 쿼리의 양이 큰데 비해 사용자의 접근이 잦아 서버 부하가 커지는 문제.  
+ 해결 : Spring cache와 통합된 Hazelcast의 분산 캐싱을 통해 5분 단위의 캐시를 걸어 해결.  
+ 결과 : 테스트 결과 캐시 미스 시 1080ms 정도의 응답 시간이 캐시 히트 이후부턴 38ms 가량으로 획기적으로 감소.  

### 기술적 의사결정 및 문제해결  

**매번 Vue, nginx, Spring을 띄워서 확인하기가 번거로운 문제**  
&nbsp;  
+ 문제 : API 맵핑을 포함한 통합테스트 과정 중 프론트, 백에 변경이 생길 때마다 어느 부분은 핫 리로딩이 되지만 어떤 부분은 되지 않아 결국 모든 파트를 다시 띄워야 하는 문제 발생  
+ 해결 : 프로젝트에 Docker를 도입하기로 결정, 각 파트를 컨테이너화 하여 Docker compose로 관리  
+ 결과 : Docker compose 명령어 한 번으로 모든 파트를 리로딩 가능, Docker의 이미지 레이어 별 캐싱으로 인해 변경이 생긴 부분만 build하여 효율적인 리로딩, 격리성을 확보하고 환경 별 설정 분리와 배포가 용이해지는 등 많은 개선이 가능해짐  
&nbsp;  

**기존에 사용하던 Github Action을 사용하지 못하게 된 문제**  
&nbsp;  
+ 문제 : 기존에 Github 플랫폼을 사용하며 Github Action을 통해 CI/CD 파이프라인을 구축했지만, 새 프로젝트가 Github가 아닌 Gitlab을 사용하게 되면서 사용이 불가능해진 문제  
+ 고민 : 새 CI/CD 툴로 기존의 Github Action과 사용법이 비슷해보이는 Gitlab runner VS 러닝커브가 조금 있지만 생태계가 강력하고 실무에서 자주 사용되는 Jenkins  
+ 선택 : 배우는 입장에서 했던 것을 또 하는 것보다는 러닝커브가 좀 있어도 실무에서 자주 사용되는 강력한 툴을 배우는 것이 장기적으로는 더 이득일 것이라 생각되어 Jenkins를 선택  
+ 결과 : gradle.yml 설정 하나만으로 끝내던 시절과 많은 것이 달라 파이프라인 구성에 시간이 좀 걸렸지만, Jenkins의 plugin을 사용해 CI/CD 파이프라인에 Sonarqube를 통합하여 더 퀄리티 있는 파이프라인을 구축하는데 성공  

&nbsp;  

**서버가 한 대 뿐이라 생긴 문제**  
&nbsp;  
+ 문제 : 지급받은 서버가 2.30GHz 4코어 cpu, 16gb ram, ssd 300gb 스펙의 EC2 서버 한 대 뿐이라 DB 서버 분리도 안되고, CI/CD 툴도 같은 서버를 사용해야 하는 문제. 이러면 CI/CD 툴이 어플리케이션과 자원을 경합해야 하고, 보안 상의 문제도 생긴다. 확인 결과 Jenkins+Sonarqube를 띄워놓기만 해도 16gb 메모리에서 4.4gb를 점유하고, Jenkins 빌드 시에는 CPU 사용량이 98%까지 순간적으로 치솟는 것을 확인할 수 있었다.  

+ 해결 : 교육 받는 곳에서 서버를 사비를 들여 추가로 기용하는 것도 뭔가 아닌 것 같아 일단 한 대로 어떻게든 사용해야 했다. 지금은 개발 과정이기 때문에 Build가 잦지만 배포 후에는 급하게 빌드할 상황이 많지 않을 것이라 판단하여 Jenkins에서 cron을 사용하여 새벽 2시에 빌드하는 nightly build 방식 채택.

+ 결과 : CI/CD 파이프라인은 유지하면서 어플리케이션의 운용에는 영향을 미치지 않도록 해결 완료.  

&nbsp;  

**분산 캐싱을 구현하는 툴 문제**  
&nbsp;  
+ 문제 : 현재 서버에 Jenkins, Sonarqube, PostgreSQL 등 많은 것이 올라가 있음에도 분산 캐싱 툴을 추가해야 하는 문제  
+ 고민 : 실무에서 많이 사용되고 다양하고 강력한 기능을 제공하지만 서버에 추가로 띄워야 하는 Redis VS JVM 내부에 노드를 생성하는 구조로 작동하여 추가로 컨테이너를 띄우지 않아도 되지만 마이너하고 정보가 많이 없는 Hazelcast  
+ 선택 : Redis의 경우 러닝커브도 어느정도 있고, 여기서 추가로 컨테이너를 더 띄우고 싶지 않아 Hazelcast로 결정. 프로젝트 규모가 조금 더 컸다면 Redis를 선택했겠지만 현재 스프링 인스턴스가 3개 뿐인 점을 감안해서 결정.  
+ 결과 : 분산 캐싱, 분산 락이 테스트 결과 Hazelcast로도 인스턴스끼리 잘 작동하는 것을 확인. 현재 서비스 규모가 크지 않고 그에 따라 캐시 규모도 크지 않아 JVM 내부의 힙 메모리를 사용하는 Hazelcast의 구조로도 무리 없이 잘 작동하는 것을 확인. Redis의 러닝커브, 컨테이너가 추가됨으로써 생기는 추가적인 관리의 복잡함 등을 감안하면 실질적인 오버헤드가 비슷하더라도 더 나은 선택을 했다고 생각함.  

&nbsp;  




### 알고리즘 

[![Solved.ac Profile](http://mazassumnida.wtf/api/v2/generate_badge?boj=rabent0207)](https://solved.ac/rabent0207/)

기본적인 문제 해결 능력을 키우기 위해 학부 시절부터 계속 배워온 C++로 백준, 프로그래머스, SWEA 등 다양한 플랫폼에서 알고리즘 풀이를 하고 있습니다.  
[링크](https://rabent.github.io/archive/)를 클릭하시면 공부하며 포스팅한 내용들을 보실 수 있습니다.