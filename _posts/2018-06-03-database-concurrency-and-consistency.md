---
layout: post
title: "데이터베이스 동시성 및 일관성"
description: "트랜잭션과 격리성 수준 그리고 Lock"
date: 2018-06-03
tags: [database, concurrency, consistency]
categories: [database]
comments: true
share: true
---

### Lock
- 공유 Lock
- 배타 Lock
- Lock Escalation
  - 관리할 Lock 리소스가 정해진 임계치를 넘으면서 Row Lock에서 Table Lock까지 점점 확장 되는 현상
  - Locking Level이 낮을수록 동시성이 증가하지만 관리해야 할 Lock 개수가 증가해 더 많은 리소스를 소비함

### 트랜잭션
- 원자성(Atomicity)
  - 작업의 최소단위
  - 전부 처리되거나 하나도 처리되지 않아야 함
- 일관성(Consistency)
  - 트랜잭션 실행의 결과로 데이터베이스 상태가 모순되지 않아야 함
- 격리성(Isolation)
  - 실행 중인 트랜잭션의 중간결과를 다른 트랜잭션이 접근할 수 없음
- 영속성(Durability)
  - 트랜잭션이 성공적으로 완료되면 영속적으로 저장됨

### 트랜잭션 격리성 수준
- Read Uncommitted
  - 아직 커밋되지 않은 데이터를 다른 트랜잭션이 읽는 것을 허용
  - Dirty Read
    - 아직 커밋되지 않은 데이터를 읽는 것
- Read Committed
  - 트랜잭션이 커밋되어 확정된 데이터만 다른 트랜잭션이 읽도록 허용
  - 레코드를 읽은 후 공유 Lock을 해제
  - Non-Repeatable Read
    - 트랜잭션 내 같은 쿼리 두 번 수행 시 결과가 다른 것
- Repeatable Read
  - Non-Repeatable Read 방지
  - 트랜잭션이 커밋될 때까지 공유 Lock을 유지
  - Phantom Read
    - 트랜잭션 내 없던 레코드가 나타나는 현상
- Serializable Read
  - Phantom Read 방지

### 동시성 제어
- 낙관적 동시성 제어
  - 같은 데이터를 동시에 수정하지 않을 것이라고 가정
  - 수정 시점에 변경 여부를 검사
- 비관적 동시성 제어
  - 같은 데이터를 동시에 수정할 것이라고 가정
  - select for update
  - select 시점에 Lock을 걸어 동시성이 감소

### 문장 수준 읽기 일관성
- 다른 트랜잭션이 데이터 추가, 변경, 삭제를 하더라도 단일 SQL내에 일관성 있게 읽는 것
- 일관성의 기준 시점은 쿼리 시작 시점
- Undo 블록을 이용한 CR Copy
- Repeatable Read 모드를 사용하게 되면 동시성이 저하됨

### 트랜잭션 수준 읽기 일관성
- 다른 트랜잭션이 데이터 추가/변경/삭제를 하더라도 트랜잭션 내에서 일관성 있게 읽는 것
- Serializable Read
