---
layout: post
title: "스프링 부트 입문기"
description: "스프링 프레임워크와 스프링 부트"
date: 2018-06-01
tags: [spring boot, spring framework]
categories: [spring boot]
comments: true
share: true
---

### 스프링 프레임워크의 특징
- DI
- AOP
- IoC
[spring framework](https://hyunwoo-e.github.io/spring-framework/)

### XML vs Java Configuration
스프링은 분명 매우 훌륭한 프레임워크이다. 하지만 방대한 XML 설정은 난이도가 높은 작업이며 컴파일이 안되어 문제 발생시 해결에 어려움이 있었다. 이러한 이유로 XML을 걷어내고 Java Configuration으로 전환하는 움직임이 나타났다.

### 스프링 부트 특징
- Embedded container - 내장된 웹 컨테이너
- Actuator - 상태 체크 및 앱 관리
- Auto Config - 필요한 라이브러리의 의존성을 안전한 버전으로 조합하여 미리 준비된 구성으로 제공
  - @SpringBootApplication으로 web.xml을 구성할 필요가 없어짐

#### @SpringBootApplication
  - @Configuration, @EnableAutoConfiguration, @ComponentScan이 결합된 애노테이션
  - @EnableAutoConfiguration
    - 스프링 부트 자동 구성의 핵심(proxy pattern)
    - 클래스패스, 애노테이션 등을 참고하여 필요한 빈을 유추하고 구성
    - exclude 옵션을 통해 자동 구성 제외

### 기본 아티팩트
- spring-boot-starter-parent
  - spring-core를 비롯한 스프링 부트 상위 의존체
- spring-boot-starter
  - spring-web, spring-webmvc, embedded tomcat 등의 라이브러리 포함

### 스프링 부트 앱 구성
  > @Value("프로퍼티명")으로 가져옴
  - application.properties
  - application.yml
  - 환경 변수
  - arguments

### 스프링 부트에서 스프링 기술 사용하기
  - @Enable<기술명>

### 기타 애노테이션
  - @Profile - 프로파일을 켰을때만 자동 구성 실행
