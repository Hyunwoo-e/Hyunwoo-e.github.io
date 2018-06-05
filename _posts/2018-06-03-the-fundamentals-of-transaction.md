---
layout: post
title: "트랜잭션의 원리"
description: "오라클 DBMS 구조를 통해 바라본 트랜잭션의 원리"
date: 2018-06-05
tags: [database, transaction]
categories: [database]
comments: true
share: true
---

### SGA
오라클이 데이터를 읽거나 변경하기 위해 사용하는 공용 메모리 영역
- Shared Pool
- Database Buffer Cache
- Redo Log Buffer
- Large Pool

### PGA
각각의 서버 프로세스가 독자적으로 사용하는 오라클 메모리 영역
- Sort Area
  - 정렬 수행을 위한 메모리 공간
  - 메모리 공간이 부족하면 디스크를 이용
- Session Information
- Cursor State
  - SQL 파싱 정보
- Stack Space
  - 바인드 변수 저장


### UNDO/REDO
- UNDO 목적
  - 트랜잭션 롤백
  - 트랜잭션 복구
  - 문장 수준 읽기 일관성

- REDO 목적
  - 복구
  - 정합성

### Write-Ahead Logging 이해
> 트랜잭션이 정상적으로 종료 처리되기 위해서는 먼저 REDO 정보가 로그에 써져야 한다. 역시 어떤 경우에도 REDO 복구를 할 수 있기 위해서는 REDO 로그가 적어도 커밋 시점에는 써져야 한다.

1. (LGWR)Database Buffer Cache를 갱신하기 전 Redo Log Buffer에 기록
2. (LGWR)Redo Log Buffer의 내용을 Redo Log File에 기록
3. (DBWR)Database Buffer Cache의 Dirty 블록을 디스크에 갱신

### 백그라운드 프로세스
- LGWR
  - Redo Log Buffer의 내용을 Redo Log File에 저장
- DBWR
  - Database Buffer Cache의 내용을 데이터 파일에 저장
  - Deffered Write + Fast Commit
    - 데이터 파일의 데이터 블록은 메모리로 복사한 후 메모리에서 읽기/변경을 수행
    - 데이터 블록이 변경될 경우 로그 정보만 즉시 기록(LGWR)하며 데이터 파일은 Batch방식으로 일괄 수행
    - 디스크 기록은 Random 액세스라 느리지만 로그는 Append 방식이므로 상대적으로 빠름

### 트랜잭션 복구 원리
1. Redo 로그에 저장된 기록으로 마지막 체크포인트이후 부터 사고 직전까지 수행되었던 트랜잭션을 재현
   - 버퍼캐시에만 수정하고 데이터파일에는 반영되지 않았던 변경사항들이 복구됨
2. UNDO 데이터를 이용해 Commit되지 않았던 트랜잭션을 모두 Rollback
   - 데이터 파일에는 커밋한 데이터만 남아 동기화됨
