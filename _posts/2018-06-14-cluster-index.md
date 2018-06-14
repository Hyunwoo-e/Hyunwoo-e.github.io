---
layout: post
title: "클러스터 인덱스 vs 비 클러스터 인덱스"
description: "클러스터 인덱스란 무엇인가?"
date: 2018-06-14
tags: [database, index, cluster]
categories: [database]
comments: true
share: true
---

### 비 클러스터 인덱스
- 특징
  - 테이블에 여러개가 존재할 수 있음
  - 레코드는 물리적으로 정렬되지 않은 구조로 저장됨
- 구조
  - B+tree 자료 구조
  - 리프 노드에는 데이터 행에 대한 포인터가 저장

### 클러스터 인덱스(=Key Cluster)
- 특징
  - 테이블당 하나만 존재함
  - PK 값이 비슷한 레코드끼리 묶어서 저장하는 것
  - PK 값에 따라 레코드의 물리적인 저장 위치를 결정
  - Primary Key가 없다면 Unique Key 또는 Row ID 등을 사용
- 구조
  - B+tree 자료 구조
  - 리프 노드에는 레코드(block/page)가 저장됨

### 클러스터 인덱스 성능
- PK 기반 검색 성능이 상대적으로 빠름
- 레코드 저장이나 PK 변경은 상대적으로 느림
- 모든 보조인덱스들이 클러스터 키를 포함하므로 크기가 커짐
