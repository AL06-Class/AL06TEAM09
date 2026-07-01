# 데이터 가이드

더미 데이터와 DB 연결 기준을 정리하는 문서입니다.

## 목적

- 역할별 화면이 같은 데이터 기준을 사용하게 합니다.
- 더미 데이터와 실제 DB 구조가 크게 달라지지 않게 합니다.
- AI가 화면마다 임의의 데이터 구조를 만들지 않게 합니다.

## 기본값

### 핵심 전제

- 현재 선택된 유저의 `role`을 기준으로 화면을 분기합니다.
- 데이터는 역할별 화면에서 필요한 정보만 필터링해서 보여줍니다.

### 역할 기준

역할 값은 아래를 기본으로 합니다.

- `center`: 센터(센터장)
- `caregiver`: 요양보호사

역할 이름을 바꾸거나 새 역할을 추가하려면 먼저 팀 논의가 필요합니다.

### 데이터 작성 기준

- 필드명은 영어 `camelCase`를 사용합니다.
- 각 데이터에는 가능하면 `id`, `createdAt`, `updatedAt`을 포함합니다.
- 날짜는 문자열 또는 Firebase Timestamp 중 하나로 통일합니다.
- 상태값은 자유 텍스트가 아니라 정해진 값만 사용합니다.
- 화면별로 같은 의미의 필드를 다른 이름으로 만들지 않습니다.

### 공통 이름 사전 운영

- DB를 처음부터 완성하지 않고, 공통으로 부를 이름만 먼저 맞춥니다.
- 새 컬렉션, 필드, 상태값, 역할 값이 필요하면 구현 전에 이 문서에 먼저 추가합니다.
- 기존 이름과 의미가 같으면 새 이름을 만들지 않습니다.
- 기능별 임시 더미 데이터는 최소로 만들고, 공통 이름은 이 문서의 사전을 따릅니다.
- 아직 확정되지 않은 이름은 초안으로 표시하고, 실제 구현이 반복되면 최종 결정에 반영합니다.

### 더미 데이터

- 더미 데이터는 기능 검증에 필요한 최소만 만듭니다.
- 화면만 맞추기 위한 임시 필드는 나중에 제거 여부를 표시합니다.
- Firebase 연결 전에도 더미 데이터만으로 역할별 화면을 확인할 수 있어야 합니다.

### DB 연결 기준

- DB 구조가 정해지기 전에는 화면 컴포넌트와 데이터 접근 코드를 분리합니다.
- Firebase 연결 코드는 한 곳에서 관리합니다.
- 화면 컴포넌트는 가능한 한 데이터 형태에만 의존하게 만듭니다.

## 논의할 항목

- 주요 데이터 종류
- 컬렉션 이름
- 유저와 역할 구조
- 문서 id 생성 방식
- 필수 필드와 선택 필드
- 날짜 저장 방식
- 상태값 목록
- 더미 데이터 위치
- 화면별 필요한 데이터
- 실제 Firebase 구조와 더미 데이터 구조를 언제 맞출지
- 공통 이름 사전에 새 이름을 추가하는 승인 기준

## 공통 이름 사전

아래 이름은 기능 개발 중 같은 의미를 다르게 부르지 않기 위한 출발점입니다. 실제 DB 구조 확정은 별도 논의로 진행합니다.

### 컬렉션 이름

- `users`: 유저
- `centers`: 방문요양센터
- `caregivers`: 요양보호사
- `careRecipients`: 수급자
- `riskHouseholdCards`: 위험가구카드
- `riskRecords`: 민원·고충·취소 기록
- `turnoverRiskReports`: 이탈위험 리포트
- `substituteWorkRequests`: 대체근무 요청

### 공통 필드 이름

- `id`: 문서 또는 항목 식별자
- `userId`: 유저 식별자
- `centerId`: 센터 식별자
- `caregiverId`: 요양보호사 식별자
- `careRecipientId`: 수급자 식별자
- `riskHouseholdCardId`: 위험가구카드 식별자
- `riskRecordId`: 민원·고충·취소 기록 식별자
- `substituteWorkRequestId`: 대체근무 요청 식별자
- `status`: 상태값
- `title`: 제목
- `description`: 설명
- `startedAt`: 시작 시각
- `endedAt`: 종료 시각
- `scheduledAt`: 확정 시각
- `source`: 생성 또는 판단 근거
- `createdAt`: 생성 시각
- `updatedAt`: 수정 시각

### 과업별 공통 필드 이름

#### 위험가구카드

- `riskLevel`: 위험등급
- `recordType`: 기록 유형
- `recordedAt`: 기록 시각
- `recordedBy`: 기록 작성자
- `complaintCount`: 민원 횟수
- `cancellationCount`: 취소 횟수

#### 이탈위험 리포트

- `reportMonth`: 리포트 기준 월
- `careTemperature`: 케어온도
- `turnoverRiskScore`: 이탈위험 점수
- `riskReason`: 위험 판단 이유
- `previousMonthDelta`: 전월 대비 변화

#### 대체근무 요청

- `workDate`: 근무 날짜
- `workStartAt`: 근무 시작 시각
- `workEndAt`: 근무 종료 시각
- `requestMessage`: 요청 메시지
- `responseStatus`: 응답 상태

#### UX/UI 개선

- UX/UI 개선 작업은 기본적으로 새 DB 이름을 만들지 않습니다.
- 화면 상태 저장이나 사용자 피드백 데이터가 필요할 때만 공통 이름 사전에 추가합니다.

### 역할 값

- `center`: 센터(센터장)
- `caregiver`: 요양보호사

### 상태값 초안

- `draft`: 초안
- `submitted`: 제출됨
- `pending`: 대기 중
- `proposed`: 제안됨
- `confirmed`: 확정됨
- `declined`: 거절됨
- `scheduled`: 일정 확정
- `generated`: 자동 생성됨
- `selected`: 선택됨
- `completed`: 완료
- `cancelled`: 취소됨

## 데이터 모델 초안

아래는 논의 출발점입니다. 실제 서비스 기획에 맞게 수정합니다.

### users

- `id`
- `name`
- `role`
- `title`
- `createdAt`
- `updatedAt`

### centers

- `id`
- `name`
- `phone`
- `address`
- `createdAt`
- `updatedAt`

### caregivers

- `id`
- `centerId`
- `name`
- `phone`
- `careTemperature`
- `status`
- `createdAt`
- `updatedAt`

### careRecipients

- `id`
- `centerId`
- `name`
- `address`
- `riskLevel`
- `createdAt`
- `updatedAt`

### riskHouseholdCards

- `id`
- `centerId`
- `careRecipientId`
- `caregiverId`
- `riskLevel`
- `status`
- `createdAt`
- `updatedAt`

### riskRecords

- `id`
- `centerId`
- `careRecipientId`
- `caregiverId`
- `recordType`
- `description`
- `recordedAt`
- `recordedBy`
- `createdAt`
- `updatedAt`

### turnoverRiskReports

- `id`
- `centerId`
- `reportMonth`
- `caregiverId`
- `careTemperature`
- `turnoverRiskScore`
- `riskReason`
- `previousMonthDelta`
- `createdAt`
- `updatedAt`

### substituteWorkRequests

- `id`
- `centerId`
- `careRecipientId`
- `caregiverId`
- `workDate`
- `workStartAt`
- `workEndAt`
- `status`
- `requestMessage`
- `responseStatus`
- `createdAt`
- `updatedAt`

## 최종 결정

- 주요 컬렉션: `users`, `centers`, `caregivers`, `careRecipients`, `riskHouseholdCards`, `riskRecords`, `turnoverRiskReports`, `substituteWorkRequests`
- 역할 기준: MVP에서는 `center`, `caregiver` 두 가지만 사용
- 필드명 규칙: 영어 `camelCase`
- 날짜 저장 방식: 문자열 또는 Firebase Timestamp 중 하나로 통일
- 상태값 기준: 자유 텍스트가 아니라 정해진 값만 사용
- 더미 데이터 기준: 기능 검증에 필요한 최소만 작성
- DB 연결 기준: 화면 컴포넌트와 데이터 접근 코드를 분리하고 Firebase 연결 코드는 한 곳에서 관리
- 공통 이름 사전 기준: 새 컬렉션, 필드, 상태값, 역할 값은 구현 전에 이 문서에 먼저 추가

## 변경 이력

- 2026-06-30: 최신 제품기획 워크시트 기준으로 케어온도 필드와 보호사관리 흐름 반영
- 2026-06-30: MVP 역할을 센터(센터장)와 요양보호사 2개로 단순화
- 2026-05-29: SPA와 역할 기반 데이터 기준 반영
- 2026-05-29: 기본 데이터 기준을 최종 결정에 반영
- 2026-05-29: 공통 이름 사전과 데이터 이름 추가 절차 반영
- 2026-05-29: 일정 조율, 면접 질문 생성, 평가 과업에 필요한 공통 이름 보강
