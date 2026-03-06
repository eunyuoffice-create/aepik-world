# GRPB01 공구 목록

## 메타 정보

| 항목 | 내용 |
|------|------|
| **화면코드** | GRPB01 |
| **화면명** | 공구 목록 |
| **라우트** | /groupbuy |
| **이용자** | PC / Mobile |
| **버전** | 0.2 |
| **작성일** | 2026-03-06 |
| **상태** | 초안 |

---

## 화면 개요

> 공동구매(일반공구/세트공구) 목록을 탐색하는 화면. 진행중/마감 탭으로 상태를 나누고, 참여 현황과 마감일을 한눈에 확인할 수 있다.

---

## 진입 경로

- 하단 네비게이션 > 홈 탭 (공구는 별도 탭이 없음)
- 홈 > 인기 공구 > 더보기
- 검색 결과

---

## 화면 구성 요소

### ① Header
- 좌측: 페이지 제목 "공구"
- 우측: 검색 아이콘(`Search`) — 탭 시 검색 페이지(SRCH01) 이동

### ② 카테고리 필터
- 가로 스크롤 형태 (모바일에서 좌우 스와이프)
- 필터 항목: 전체 / 애니/만화/소설 / 아이돌/아티스트 / 배우/무대 / 게임
- 선택된 필터: Blue 배경 + 흰색 텍스트
- 각 카테고리 탭 시: 선택 + 목록 필터링

### ③ 탭 바 (Status Tabs)
- 모집중 (기본 활성) / 진행중 / 완료·무산
- 모집중: status=recruiting인 공구 표시
- 진행중: status=pending_decision, confirmed, shipping인 공구 표시
- 완료·무산: status=completed, cancelled인 공구 표시
- 탭 탭 시: 목록 리로드

### ④ 필터 칩 바
- 가로 스크롤 칩 형태
- 필터 항목: 작품 ▾ / 굿즈 타입 ▾ / 공구 유형 ▾ (일반/세트)
- 각 칩 탭 시: 바텀시트 → 옵션 선택 → 칩에 선택값 표시
- 초기화 버튼: 선택된 필터가 있을 때만 표시

### ⑤ 정렬 + 결과 수
- 좌측: 전체 N개 (검색 결과 수)
- 우측: 정렬 드롭다운 — 최신순(기본) / 인기순 / 마감임박순
- 정렬 변경 시 목록 리로드

### ⑥ 공구 카드 리스트
- 1열 리스트 형태 (NOT 그리드)
- 카드 구성: GroupBuyCard 컴포넌트
  - 이미지 (16:9 비율, object-fit: cover)
  - 공구 타입 태그 ([공구] 또는 [세트공구]) + 카테고리 태그
  - 상품명 (최대 2줄, text-overflow: ellipsis)
  - 진행자 닉네임 + 신뢰배지 (Lv.1/Lv.2/Lv.3)
  - 참여현황 N/M명 + 프로그레스바 (80% 이상 = 녹색, 미만 = 파란색)
  - 마감일 D-N (디데이 표시)
  - 가격 ₩15,000/1인 (1인당 가격)
- 마감된 공구: "마감" 뱃지 + 반투명 처리
- 목표 미달 공구: "목표 미달" 뱃지 표시
- 진행 결정 대기 공구: "진행 결정 대기" 뱃지 (Amber/노란색) 표시
- 무산 공구: "무산" 뱃지 (Red) + 취소선 + 반투명 처리
- 진행 확정 공구: "진행 확정" 뱃지 (Green) 표시
- 배송중 공구: "배송중" 뱃지 (Blue) 표시
- 무한 스크롤 (Intersection Observer)
- 스켈레톤 UI: 로딩 시 카드 형태 회색 박스 펄스 애니메이션

### ⑦ FAB (Floating Action Button)
- 우하단 고정 (bottom: 80px, right: 16px)
- 아이콘: + — 공구 등록
- 비로그인 시: 로그인 유도 팝업
- 스크롤 다운 시 축소/반투명

### ⑧ 하단 네비게이션 (BottomNav)
- 5개 탭: 홈(활성) | 공구(구별 안함) | 이벤트 | 채팅 | 마이
- 홈 탭 활성 상태 (Blue)

### ⑨ Empty State
- 조건: 필터 결과 없음 or 아직 등록된 공구가 없음
- 메시지: "조건에 맞는 공구가 없어요"
- 일러스트: 빈 상자 아이콘
- CTA: "공구 등록하기" 버튼

---

## 데이터 필드

### API: GET /api/groupbuy

**Request (Query Parameters):**

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| page | number | X | 페이지 번호 (기본 1) |
| limit | number | X | 페이지당 개수 (기본 20) |
| category | string | X | 카테고리 필터 (anime_manga/idol_artist/actor_stage/game) |
| work_id | string | X | 작품 필터 |
| goods_type | string | X | 굿즈 타입 필터 |
| type | string | X | 공구 유형 (normal/set) |
| status | string | X | 상태 (active/closed) |
| sort | string | X | 정렬 (latest/popular/deadline) |

**Response:**

| 필드 | 타입 | 설명 |
|------|------|------|
| items | array | 공구 목록 |
| items[].id | string | 공구 ID |
| items[].images | string[] | 이미지 URL 배열 |
| items[].title | string | 상품명 |
| items[].type | string | 공구 유형 (normal/set) |
| items[].price_per_person | number | 1인당 가격 |
| items[].target_count | number | 목표 인원 |
| items[].current_count | number | 현재 참여 인원 |
| items[].deadline | string | 마감일 (ISO 8601) |
| items[].delivery_date | string | 배송 예정일 (ISO 8601) |
| items[].status | string | 상태 (recruiting/pending_decision/confirmed/cancelled/shipping/completed) |
| items[].category | string | 카테고리 |
| items[].organizer | object | 진행자 {nickname, trust_level, trades} |
| items[].created_at | string | 등록일 |
| total | number | 전체 개수 |
| has_next | boolean | 다음 페이지 존재 여부 |

---

## 비즈니스 로직

| # | 규칙 | 설명 |
|---|------|------|
| 1 | 자동 마감 | 마감일 도래 시 자동으로 status=pending_decision 전환 |
| 2 | 진행 결정 대기 | 마감일 도래 시 자동 pending_decision 전환, "진행 결정 대기" 뱃지 |
| 3 | 진행자 자격 | 공구 등록 권한: 교환 5회 이상 + 긍정 평가 90% 이상 |
| 4 | 마감 공구 표시 | 마감된 공구는 상태별 뱃지 표시 후 목록 하단 이동 (또는 숨김) |
| 5 | 정렬 기본값 | 최신순 (created_at DESC) |
| 6 | 참여도 표시 | current_count / target_count로 프로그레스바 표시 (80% 이상 녹색, 미만 파란색) |
| 7 | 자동 무산 | 진행 결정 대기 D+5일 미결정 시 자동 무산 |
| 8 | 상태 뱃지 색상 | 모집중=Blue, 진행결정대기=Amber, 진행확정=Green, 무산=Red, 배송중=Blue, 완료=Gray |

---

## 인터랙션

| # | 트리거 | 동작 | 비고 |
|---|--------|------|------|
| 1 | 카드 탭 | 공구 상세 페이지 이동 | /groupbuy/[id] → GRPB02 |
| 2 | 카테고리 필터 탭 | 필터 적용 → 목록 리로드 | 다중 선택 불가 (단일 선택) |
| 3 | 상태 탭 (진행중/마감) | 탭 전환 → 목록 리로드 | |
| 4 | 필터 칩 탭 | 바텀시트 열림 → 옵션 선택 | 다중 선택 가능 |
| 5 | 정렬 탭 | 드롭다운 → 옵션 선택 → 목록 리로드 | |
| 6 | FAB 탭 | 공구 등록 페이지 이동 | 미로그인 시 로그인 유도 → GRPB03 |
| 7 | 스크롤 하단 도달 | 다음 페이지 로드 | 스켈레톤 표시 |
| 8 | 풀투리프레시 | 목록 새로고침 | 모바일만 |
| 9 | 뒤로가기 | 홈 또는 이전 페이지 | |

---

## 연관 화면

| 이동 대상 | 조건 | 화면코드 |
|----------|------|---------|
| 공구 상세 | 카드 탭 | GRPB02 |
| 공구 등록 | FAB 탭 | GRPB03 |
| 검색 | 검색 아이콘 탭 | SRCH01 |
| 홈 | 뒤로가기 / 홈 탭 | HOME01 |
| 로그인 | 비로그인 + FAB | AUTH01 |

---

## 개정 이력

| 날짜 | 버전 | 내용 | 작성자 |
|------|------|------|--------|
| 2026-03-06 | 0.1 | 초안 작성 | - |
| 2026-03-06 | 0.2 | 공구 상태 시스템 (진행결정대기/무산 등) 뱃지 추가 | - |
