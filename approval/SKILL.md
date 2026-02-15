---
name: approval
description: 포킷워크 결재 도메인. 결재선 규칙, 결재 요청, 결재 단계, 알림의 엔티티 정의와 비즈니스 규칙. 결재 관련 기능 구현 시 참고한다.
---

# 결재관리

## 결재선 규칙 (Approval Chain Rule)

### 엔티티

- 부서 (부서당 1개)
- 결재자 목록 (순서 보장)

### 비즈니스 규칙

- 부서별로 결재선을 설정한다
- 결재자는 순서대로 배치된다 (1차 결재자, 2차 결재자, ...)
- 결재선이 없는 부서의 구성원은 연차 신청 시 결재 없이 처리될 수 있다
- 빈 결재자 목록으로 설정하면 결재선이 삭제된다

## 결재 요청 (Approval Request)

### 엔티티

- 신청자 (구성원)
- 유형: 연차 | 지출 | 출장 | 경조사 | 교육
- 제목, 설명
- 상태: 임시보관 | 대기 | 진행중 | 완료 | 반려
- 유형별 부가 정보:
  - 연차/출장: 시작일, 종료일, 일수
  - 지출/출장/경조사/교육: 금액
  - 출장: 목적지
  - 경조사: 경조사 유형
  - 교육: 교육명, 교육기관
- 메모
- 수신참조 대상자 목록

### 상태 전환

```
임시보관 → 대기 (상신)
대기 → 진행중 (1차 결재자 확인)
진행중 → 완료 (모든 결재자 승인)
대기/진행중 → 반려 (결재자 반려)
```

### 비즈니스 규칙

- 연차 신청 시 결재 요청이 자동 생성된다
- 결재는 결재선 순서대로 진행된다 (1차 → 2차 → ...)
- 앞 단계가 승인되어야 다음 단계가 활성화된다
- 한 단계에서 반려되면 전체 요청이 반려된다

## 결재 단계 (Approval Step)

### 엔티티

- 결재 요청
- 결재자 (구성원)
- 순서
- 상태: 대기 | 승인 | 반려
- 코멘트
- 처리 일시

## 알림

### 비즈니스 규칙

- 결재 요청 시 결재자에게 알림이 발송된다
- 승인/반려 시 신청자에게 알림이 발송된다

## 스키마

```mermaid
erDiagram
    members {
        uuid id PK
        varchar name
    }

    departments {
        uuid id PK
        varchar name
    }

    approval_requests {
        uuid id PK
        uuid requester_id FK
        varchar type "연차 | 지출 | 출장 | 경조사 | 교육"
        varchar title
        text description
        varchar status "임시보관 | 대기 | 진행중 | 완료 | 반려"
        date start_date "nullable"
        date end_date "nullable"
        decimal days "nullable"
        int amount "nullable"
        varchar destination "nullable"
        varchar event_type "nullable"
        varchar course_name "nullable"
        varchar institution "nullable"
        text memo "nullable"
        timestamp created_at
    }

    approval_steps {
        uuid id PK
        uuid request_id FK
        uuid approver_id FK
        int step_order
        varchar status "대기 | 승인 | 반려"
        text comment "nullable"
        timestamp acted_at "nullable"
    }

    approval_cc {
        uuid id PK
        uuid request_id FK
        uuid member_id FK
    }

    approval_chain_rules {
        uuid id PK
        uuid department_id FK "UK - 부서당 1개"
        timestamp created_at
    }

    approval_chain_rule_steps {
        uuid id PK
        uuid rule_id FK
        uuid approver_id FK
        int step_order
    }

    approval_requests }o--|| members : "requested by"
    approval_steps }o--|| approval_requests : "belongs to"
    approval_steps }o--|| members : "approver"
    approval_cc }o--|| approval_requests : "cc of"
    approval_cc }o--|| members : "cc to"
    approval_chain_rules ||--|| departments : "for"
    approval_chain_rule_steps }o--|| approval_chain_rules : "belongs to"
    approval_chain_rule_steps }o--|| members : "approver"
```
