---
title: "DTO, VO, Entity의 개념과 차이"
layout: post
date: 2024-10-18 17:41
tag:
- Spring
- JAVA
description: 토막 상식
---  

# DTO, VO, Entity  
토이 프로젝트를 위해 이것저것 검색하고 기획하던 중 강의에선 보지 못했던 전혀 처음 보는 단어들이 나왔다. 심지어 그 단어들은 넷 상의 여러 포스팅에서 기본적으로 다들 사용하기도 했다.  
기본적이고 중요한 개념같으니 이 참에 알아보고 정리해보기로 했다.  

## DTO  
**Data Transfer Object**, DTO는 이름 그대로 데이터를 전송하기 위해 만들어진 객체이다.  
현대의 프로그램은 프로그램의 계층을 presentation, domain 등의 레이어로 나누고, 지금까지 배워왔던 스프링의 MVC 구조도 각각의 계층을 나눠 의존관계를 최소화하는데 초점이 맞춰져 있다.  
DTO는 이러한 배경에서 **각 계층간의 데이터 전송을 위한 객체**이다. Controller와 Service 사이를, Controller와 View 사이를 오가는 데이터 전송에서 사용할 수 있다.  
DTO를 사용하는 것엔 여러 장점이 있는데, 계층 간 데이터 교환에서 **불필요한 데이터가 들어가지 않게** 할 수 있고, 각 **레이어의 구분을 더 명확하게** 할 수 있다.  
어디까지나 계층 간 데이터 전송이 핵심이라는 것을 기억하면 될 것 같다.  

## Entity  
**Entity**는 실제 DB의 relation과 1:1로 매칭되는 객체이다. 그러므로 DB relation에 있는 attribute만 필드로 가져야 한다.  
Entity의 가장 큰 특징은 **영속성**이다. 따라서 setter의 사용을 지양하고 대신 생성자 등을 사용해야 한다.  
사실상 DB의 테이블을 객체로써 그대로 가지고 나온 것이라고 보면 될 것 같다. Service, Controller 등에서 이 Entity 객체를 기준으로 모든 로직을 진행한다.  
DB와의 매칭, 영속성이 핵심인 객체이므로 계층 간 정보 전달에는 사용해서는 안되고 Entity 대신 가변형이고, presentation logic을 추가 가능한 DTO로 따로 분리하여 사용한다.  

## VO  
VO(Value Object)는 **값을 표현하기 위한 객체**이다. VO는 생성자를 사용하여 불변하는 속성을 갖는다.  
VO의 핵심 기능은 **equlas(), hashcode() 메서드의 오버라이딩**에 있다. 두 객체의 필드값이 모두 같다면 같은 객체로 판단하는 것이다. 1000원짜리 화폐가 1000원이라는 값을 나타내고, 같은 1000원짜리는 같은 객체로 판단하는 것과 같다.  
이러한 VO는 Entity의 일부를 나타내는데 사용된다. 도메인의 중요한 개념이나 불변해야 할 값을 나타내어 비즈니스 로직에서 사용할 수 있다.  

# 정리  
이 세 가지 용어는 하드한 개념이 아니라 어디까지나 개발의 방법론과 같은 느낌이라 설명이 상당히 두루뭉술하다. 대충이나마 이해하는데 여러 자료를 살펴보고 교차검증해야 했다.  
토이 프로젝트를 하면서 필요한 부분이 있다면 활용하여 구현하면 좋을 것 같다. 적어도 계층 간 데이터 전달에 DTO 정도는 필수적으로 들어가는 것이 좋겠다.