---
layout: post
title: "자바의 기본"
description: "메모리 관점에서 바라본 자바의 자료 타입 그리고 자바의 예외"
date: 2018-06-03
tags: [java]
categories: [java]
comments: true
share: true
---

### 자바의 타입
- 원시 타입
  - 스택 영역에 저장
  - byte/short/int/long
  - float/double
  - boolean
  - char
- 참조 타입
  - 참조 타입에 저장되는 메모리 주소는 스택 영역
  - 그 메모리 주소가 가르키는 메모리는 힙 영역
  - 원시 타입이 아닌 모든 자료형
    - Wrapper 클래스도 참조형
    - 원시 타입의 배열형도 참조형
  - 가비지 컬렉션에 의해 소멸

### 박싱/언박싱
- 박싱 - 기본형을 참조형으로 변환
- 언박싱 - 참조형을 기본형으로 변환

### 자바의 인자 전달 방식
**자바는 항상 call by value**
- 원시 타입은 값이 복사됨
- 참조 타입은 참조가 복사됨

### String vs StringBuilder vs StringBuffer
- String
  - private final char value[]로 불변
  - 새로운 값을 할당할 때마다 새로 생성
- StringBuilder
  - memory에 append하는 방식
  - 클래스를 새로 생성하지 않음
- StringBuffer
  - StringBuilder의 메소드에 synchronized가 추가
  - thread-safe

### 자바의 예외
- Error
  - 시스템 레벨(JVM)에서 발생
  - 애플리케이션에서 처리하지 않음
- Exception
  - 체크예외
    - RuntimeException을 상속하지 않은 예외
    - 컴파일 시점에 확인
    - 반드시 예외 처리 코드를 작성해야 함
    - roll-back 하지 않음
  - 언체크예외
    - RuntimeException을 상속한 예외
    - 실행 시점에 확인
    - 예외 처리 코드를 작성하지 않아도 됨
    - roll-back 함

### 예외 처리 방법
- 예외 복구 (로직의 전환)
- 예외 회피 (호출한 대상에 예외처리를 위임)
- 예외 전환
  - 의미 있는 예외로 전환
  - 처리하기 쉬운 예외로 전환
  - 중첩 예외와 getCause()를 통해 이력 관리 가능
