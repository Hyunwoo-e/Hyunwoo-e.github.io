---
layout: post
title: "데이터베이스 성능 튜닝 핵심원리"
description: "데이터베이스 부하와 개선 방법"
date: 2018-06-03
tags: [database, performance]
categories: [database]
comments: true
share: true
---

### 데이터베이스 부하
- SQL 파싱 부하
- 데이터베이스 Call 부하
- 데이터베이스 I/O 부하

### SQL 파싱 부하
- SQL 파싱
  - 소프트 파싱 - SQL과 실행계획을 캐시에서 찾아 실행
  - 하드 파싱 - SQL과 실행계획을 캐시에서 찾지 못해 최적화 과정을 거쳐 실행
- 튜닝 포인트
  - 바인드 변수
    - 바인드 변수를 사용하면 실행계획 공유함
    - 컬럼 히스토그램을 균등분포로 가정하게 됨
  - 애플리케이션 커서 캐싱
    - 문법검사, 실행계획 검색, 메모리 할당 작업도 한번만 수행
    - statement 닫지않고 사용하거나 묵시적 캐싱옵션을 사용

### 데이터베이스 Call 부하
- 데이터베이스 Call
  - User Call – DBMS 외부로부터 요청되는 Call
  - Recursive Call – DBMS 내부에서 발생하는 Call
    - (함수, 프로시저 등 -> 바인드변수 활용)
- 튜닝 포인트
  - One SQL
    - 반복 수행 -> One SQL
    - 함수 호출 -> One SQL
  - Array Processing
    - 한 번의 SQL(INSERT/UPDATE/DELETE) 수행으로 다량의 레코드 처리
      - executeBatch 사용
  - Fetch Call 최소화
    - 부분 범위 처리
      - 최초 rs.next() 이후 ArraySize만큼은 데이터베이스 call을 발생시키지 않음
      - ArraySize가 증가하면 Fetch Call 및 Block IO가 감소
      - setFetchSize()를 사용해 ArraySize조정
    - 페이지 처리
      - 페이지 단위로 필요한 만큼만 조회

### 데이터베이스 I/O 최소화
- 블록 단위 I/O
  - DBMS에서 I/O는 블록 단위로 이루어짐
  - 하나의 레코드를 읽더라도 레코드가 속한 블록 전체를 읽음
  - 액세스하는 블록 개수는 SQL 성능을 좌우하고 옵티마이저의 판단에도 영향
- 메모리I/O vs 디스크I/O
  - 버퍼 캐시 히트율
  - 논리적인 블록 요청의 횟수를 최소화
- Sequential I/O vs Random I/O
  - Sequential 액세스에 의한 선택 비중 높이기
  - Random 액세스 발생량 줄이기
  - 최소한의 블록만 읽도록 작성 및 유도
  - Multiblock I/O

### 사용자 정의 함수 vs 내장 함수
사용자 정의 함수는 내장함수처럼 Native 코드로 완전 컴파일된 형태가 아니어서 가상머신(Virtual Machine) 같은 별도의 실행엔진을 통해 실행된다.

실행될 때마다 컨텍스트 스위칭(Context Switching)이 일어나며, 이 때문에 내장함수(Built-In)를 호출할 때와 비교해 성능을 떨어뜨린다.
