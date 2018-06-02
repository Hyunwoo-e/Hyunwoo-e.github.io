---
layout: post
title: "스프링 프레임워크"
description: "스프링 프레임워크의 핵심 원리와 특징"
date: 2018-06-01
tags: [spring framework]
categories: [spring framework]
comments: true
share: true
---

#### Bean
스프링이 제어권을 가지고 IoC 방식으로 관리하는 오브젝트

#### IoC Object
- BeanFactory
  - Bean의 생성과 관계 설정 제어를 담당하는 IoC Container
  - getBean()이 호출된 시점에서야 해당 빈을 생성(lazy loading)
- ApplicationContext
  - Bean Factory를 확장한 IoC Container
  - 컨텍스트 초기화 시점에 모든 싱글톤 빈을 미리 로드
  - 싱글톤 레지스트리
    - 스프링은 직접 싱글톤 오브젝트를 만들고 관리함  
    - 제어권을 컨테이너에게 넘김으로써 [싱글톤 패턴의 단점](https://hyunwoo-e.github.io/object-oriented-programming/)을 개선
    - public 생성자를 가질 수 있음
    - singleton이 하나만 만들어 지는 것을 보장
    > 싱글톤은 멀티쓰레드라면 동시에 접근이 가능하므로 변수들은 파라미터 변수, 지역 변수, 리턴 값 등을 이용해야 함

### 스프링 프레임워크의 특징
- IoC
  - 객체의 생성에서 생명 주기 관리의 제어권을 프레임워크가 가짐
  - @Autowired를 사용한 자동 주입, AOP 등은 IoC를 통해 가능
- DI
  - 객체간의 의존성을 외부에서 주입 받는 것
  - 느슨한 연결을 통해 결합도를 낮춤
- AOP
  - 횡단 관심사를 분리하여 객체 간 결합도를 낮추는 목적
  - 트랜잭션(Transaction), 보안(Security), 캐싱(Caching) 등
  - Spring의 AOP
    - XML 스키마
    - AspectJ에서 정의한 @Aspect 애노테이션
    - AOP 구현
  - AOP 원리
    - Proxy - 매번 새로운 클래스를 정의해야 함
    - Dynamic Proxy - 리플렉션(java.lang.reflect)을 이용한 프록시 생성

### 트랜잭션 서비스 추상화
- PlatformTransactionManager
![platformtransactionmanager](https://user-images.githubusercontent.com/20318775/40874225-a01417d2-66a7-11e8-9018-2381a6b1bbc6.png)
> 출처: https://t1.daumcdn.net/cfile/tistory/2739D5435794C72C38

  - DI를 통해 트랜잭션 클래스를 주입받음
  - 다형성을 이용해 commit/rollback을 특정 기술에 종속되지 않음
  - 트랜잭션 처리 기술이 바뀌더라도 서비스 코드의 변경이 없음
