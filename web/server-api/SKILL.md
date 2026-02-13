---
name: server-api
description: Pokitwork 서버 API 프로젝트 특화 규칙. better-auth 경로 제한, DnD 정렬 패턴, 구현된 Query Service 및 REST API 엔드포인트 카탈로그. API 엔드포인트 추가, Query Service 작성, 서버 코드 수정 시 참고한다.
---

# 서버 API — 프로젝트 특화 사항

> 아키텍처 원칙(Active Record, Query Service, 데이터 흐름, 엔드포인트 패턴)은
> `sveltekit-conventions` 스킬을 참고한다.

## 프로젝트 규칙

- **`/api/auth/*` 경로는 better-auth가 점유** — 커스텀 엔드포인트는 `/api/login/*`, `/api/admin/*`, `/api/user/*` 등 다른 경로를 사용
- **DnD 정렬**: 로컬 `$state` + `$effect` 동기화 → finalize 시 fetch 호출

## 구현된 Query Service

| 서비스 | 메서드 | 반환 타입 | 용도 |
|--------|--------|-----------|------|
| `MemberQueryService` | `listOptions()` | `MemberOption[]` | Layout 공유 — 피커/셀렉트 |
| | `listPage(params)` | `MemberPage` | 구성원 탭 — 서버 페이징 |
| `LeaveQueryService` | `listRecordPage(params)` | `LeaveRecordPage` | 연차 현황 — 서버 페이징 |
| | `listHistory(memberId, year)` | `LeaveHistoryResponse` | 사용 내역 + 조정 이력 |
| `OrganizationQueryService` | `listDepartmentOptions()` | `DepartmentOption[]` | Layout 공유 — 드롭다운 |
| | `listDepartments()` | `DepartmentView[]` | 부서 탭 — 플랫 리스트 |
| | `listPositionOptions()` | `{ id, name }[]` | Layout 공유 — 드롭다운 |
| | `listPositions()` | `Position[]` | 직책 탭 |
| `ApprovalQueryService` | `listRequests(params)` | `ApprovalRequestPage` | 결재 현황 |
| | `listChainRules()` | `ApprovalChainRuleView[]` | 결재선 규칙 |
| `InboxQueryService` | `listItems(memberId)` | `InboxItemView[]` | Inbox 통합 |
| | `getApprovalDetail(requestId)` | `ApprovalDetailView` | 결재 상세 |
| `NoticeQueryService` | `listPage(params)` | `NoticePage` | 공지 관리 (admin) |
| | `listRecent(limit)` | `NoticeView[]` | 최근 공지 (홈) |
| | `listAll()` | `NoticeView[]` | 전체 공지 (사용자) |
| `DashboardQueryService` | `getStats()` | `DashboardStats` | 대시보드 통계 |
| | `listRecentApprovals(limit)` | `RecentApproval[]` | 최근 결재 |
| | `listDepartmentStats()` | `DepartmentStat[]` | 부서별 현황 |

## REST API 엔드포인트

### 인증/초대
- `POST /api/login/check-email` — 이메일 존재 확인
- `GET /api/invite/[token]` — 초대 토큰 검증
- `POST /api/invite/[token]/accept` — 초대 수락

### 관리자 — 구성원
- `POST /api/admin/members` — 구성원 추가
- `PATCH /api/admin/members/[id]` — 수정
- `DELETE /api/admin/members/[id]` — 삭제
- `POST /api/admin/members/invite` — 초대 발송

### 관리자 — 부서
- `POST /api/admin/departments` — 생성
- `PATCH /api/admin/departments/[id]` — 수정
- `DELETE /api/admin/departments/[id]` — 삭제
- `PUT /api/admin/departments/reorder` — 정렬
- `POST /api/admin/departments/[id]/members` — 구성원 배정
- `DELETE /api/admin/departments/[id]/members/[memberId]` — 해제

### 관리자 — 연차
- `POST /api/admin/leaves/adjust` — 연차 조정
- `GET /api/admin/leaves/usages` — 사용 내역

### 관리자 — 공지사항
- `POST /api/admin/notices` — 작성
- `PATCH /api/admin/notices/[id]` — 수정
- `DELETE /api/admin/notices/[id]` — 삭제
- `POST /api/admin/notice-categories` — 카테고리 생성
- `PATCH /api/admin/notice-categories/[id]` — 수정
- `DELETE /api/admin/notice-categories/[id]` — 삭제

### 사용자
- `POST /api/user/leave-requests` — 연차 신청
- `POST /api/user/approvals/[id]/decide` — 결재 승인/반려
