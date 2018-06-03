---
layout: post
title: "스프링 프레임워크 설정"
description: "톰캣 및 스프링 설정"
date: 2018-06-01
tags: [spring framework, tomcat, config]
categories: [spring framework]
comments: true
share: true
---

### Tomcat 구동 순서
1. server.xml 읽기
   - docBase와 path 등을 통해 서비스할 프로젝트를 확인
2. [web.xml](http://wiki.gurubee.net/pages/viewpage.action?pageId=26740333) 읽기

### web.xml
- context 설정
  - 이름이나 객체를 바인딩하는 역할
  - <context-param> 태그로 ContextLoaderListener 설정
- servlet 설정
  - dispatcher-servlet
  - custom-servlet
- listener 설정
  - ContextLoaderListener는 ApplicationContext를 로드하면 ServletContext 인스턴스 생성 시(톰켓으로 어플리케이션이 로드되는 시점)에 호출된다.
  - ContextLoaderListener는 WebApplicationContext을 생성
  - WebApplicationContext가 초기화 될때 dispatcher-servlet.xml에 선언한 bean이 등록됨
- filter 설정
  - 요청이 오면 서버로 보내기 전에 인코딩
- welcome list, error page 등 설정

### dispatcher-servlet.xml
- View Resolver 설정
  - 뷰 이름으로부터 사용할 뷰 오브젝트를 매핑
- component-scan
  - @Component와 하위 @Controller, @Repository, @Service 애노테이션이 명시된 클래스들을 빈으로 등록
- annotation-driven
  - @Controller와 @RequestMapping 사용 활성화
  - HandlerMapping(default: RequestMappingHandlerMapping) 등록
  - HandlerAdapter(default: RequestMappingHandlerAdapter) 등록

### Dispatcher Servlet Flow
![dispatcher_servlet](https://user-images.githubusercontent.com/20318775/40885325-11a0b092-675f-11e8-81a7-04c337886dc6.jpg)
