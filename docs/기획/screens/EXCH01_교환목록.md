# EXCH01 교환 목록

## 메타 정보

| 항목 | 내용 |
|------|------|
| **화면코드** | EXCH01 |
| **화면명** | 교환 목록 |
| **라우트** | /exchange |
| **이용자** | PC / Mobile |
| **버전** | 0.2 |
| **작성일** | 2026-03-06 |
| **상태** | 초안 |

---

## 화면 개요

> 등록된 교환글을 필터링하고 탐색하는 메인 목록 화면. 작품/캐릭터/굿즈 타입 등 다양한 조건으로 필터링하여 원하는 교환 매물을 찾을 수 있다.

---

## 진입 경로

- 하단 네비게이션 > 교환 탭
- 홈 > 인기 교환 > 더보기
- 검색 결과 > 교환 탭
- 타인 프로필 > 교환 목록

---

## 화면 구성 요소

### ① Header
- 좌측: 페이지 제목 "교환"
- 우측: 검색 아이콘(`Search`) — 탭 시 검색 페이지(SRCH01) 이동

### ② 필터 바 (FilterChips)
- 가로 스크롤 칩 형태
- 필터 항목: 작품 ▾ / 캐릭터 ▾ / 굿즈 타입 ▾ / 상품 상태 ▾ / 거래 방법 ▾ / 차액 여부 ▾
- 각 칩 탭 시: 바텀시트 → 옵션 선택 → 칩에 선택값 표시 (예: "작품" → "하이큐")
- 초기화 버튼: 전체 필터 초기화 (선택된 필터가 있을 때만 표시)

### ③ 정렬 + 결과 수
- 좌측: 전체 N개 (검색 결과 수)
- 우측: 정렬 드롭다운 — 최신순(기본) / 인기순 / 가까운순
- 가까운순은 위치 권한 허용 시에만 활성화

### ④ 교환글 그리드
- 2열 그리드 (모바일) / 4열 (데스크톱)
- 카드 구성: ProductCard 컴포넌트
  - 이미지 (1:1 비율, object-fit: cover)
  - 거래유형 태그 + 카테고리 태그
  - 상품명 (최대 2줄, text-overflow: ellipsis)
  - 작품명 · 캐릭터명 (Gray 400)
  - ★ 평점 · 거래 횟수
- 교환 완료 매물: "교환완료" 오버레이 + 반투명 처리
- 끌어올리기된 매물: `Flame` 아이콘 + "UP" 뱃지 표시
- 제안 받은 매물: "제안 N건" 뱃지 (본인 글에만 표시)
- 무한 스크롤 (Intersection Observer)
- 스켈레톤 UI: 로딩 시 카드 형태 회색 박스 펄스 애니메이션

### ⑤ FAB (Floating Action Button)
- 우하단 고정 (bottom: 80px, right: 16px)
- 아이콘: `Edit` (+) — 교환글 등록
- 비로그인 시: 로그인 유도 팝업
- 스크롤 다운 시 축소/반투명

### ⑥ 하단 네비게이션 (BottomNav)
- 5개 탭: 홈(`Home`) | **교환(`Repeat`)** | 이벤트(`Calendar`) | 채팅(`MessageCircle`) | 마이(`User`)
- 교환 탭 활성 상태 (Blue)

### ⑦ Empty State
- 조건: 필터 결과 없음 or 아직 등록된 교환글 없음
- 메시지: "조건에 맞는 교환글이 없어요"
- 일러스트: 빈 상자 아이콘
- CTA: "교환글 등록하기" 버튼

---

## 데이터 필드

### API: GET /api/exchange

**Request (Query Parameters):**

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| page | number | X | 페이지 번호 (기본 1) |
| limit | number | X | 페이지당 개수 (기본 20) |
| work_id | string | X | 작품 필터 |
| character_id | string | X | 캐릭터 필터 |
| goods_type | string | X | 굿즈 타입 필터 |
| condition | string | X | 상품 상태 (S/A/B/C) |
| trade_method | string | X | 거래 방법 (delivery/meetup/both) |
| has_cash_diff | boolean | X | 차액 여부 |
| sort | string | X | 정렬 (latest/popular/nearest) |

**Response:**

| 필드 | 타입 | 설명 |
|------|------|------|
| items | array | 교환글 목록 |
| items[].id | string | 교환글 ID |
| items[].images | string[] | 이미지 URL 배열 |
| items[].title | string | 상품명 |
| items[].work | object | 작품 정보 {id, name} |
| items[].character | object | 캐릭터 정보 {id, name} |
| items[].goods_type | string | 굿즈 타입 |
| items[].condition | string | 상품 상태 |
| items[].trade_type | string | 거래 유형 (exchange/exchange_cash) |
| items[].category | string | 카테고리 (anime_manga/idol_artist/actor_stage/game) |
| items[].user | object | 등록자 {nickname, rating, trades, trust_level} |
| items[].created_at | string | 등록일 |
| items[].status | string | 상태 (active/completed/hidden) |
| items[].proposal_count | number | 받은 제안 수 (본인 글인 경우) |
| items[].is_bumped | boolean | 끌어올리기 여부 |
| items[].bumped_at | string | 끌어올리기 시간 (nullable) |
| total | number | 전체 개수 |
| has_next | boolean | 다음 페이지 존재 여부 |

---

## 비즈니스 로직

| # | 규칙 | 설명 |
|---|------|------|
| 1 | 자동 비공개 | 등록 후 60일 미갱신 시 자동 비공개 |
| 2 | 교환 완료 매물 | 교환 완료된 매물은 "교환완료" 뱃지 표시 후 목록 하단 이동 |
| 3 | 차단 사용자 | 차단한 사용자의 매물은 표시하지 않음 |
| 4 | 신고 매물 | 3건 이상 신고된 매물은 관리자 검토 전까지 비공개 |
| 5 | 정렬 기본값 | 최신순 (created_at DESC) |
| 6 | 가까운순 | 위치 권한 허용 시에만 활성화, 직거래 가능 매물만 표시 |
| 7 | 끌어올리기 정렬 | 끌어올리기된 매물은 bumped_at 기준으로 목록 상단 노출 |
| 8 | 제안 뱃지 표시 | 본인이 등록한 매물에 대기중 제안이 있으면 "제안 N건" 뱃지 표시 |

---

## 인터랙션

| # | 트리거 | 동작 | 비고 |
|---|--------|------|------|
| 1 | 카드 탭 | 교환 상세 페이지 이동 | /exchange/[id] |
| 2 | 필터 칩 탭 | 바텀시트 열림 → 옵션 선택 | 다중 선택 가능 |
| 3 | 정렬 탭 | 드롭다운 → 옵션 선택 → 목록 리로드 | |
| 4 | FAB 탭 | 교환글 등록 이동 | 미로그인 시 로그인 유도 |
| 5 | 스크롤 하단 도달 | 다음 페이지 로드 | 스켈레톤 표시 |
| 6 | 풀투리프레시 | 목록 새로고침 | 모바일만 |
| 7 | 뒤로가기 | 홈 또는 이전 페이지 | |

---

## 연관 화면

| 이동 대상 | 조건 | 화면코드 |
|----------|------|---------|
| 교환 상세 | 카드 탭 | EXCH02 |
| 교환글 등록 | FAB 탭 | EXCH03 |
| 검색 | 검색 아이콘 탭 | SRCH01 |
| 교환 신청함 | 제안 뱃지 탭 | EXCH05 |
| 홈 | 뒤로가기 / 홈 탭 | HOME01 |
| 로그인 | 비로그인 + FAB | AUTH01 |

---

## 개정 이력

| 날짜 | 버전 | 내용 | 작성자 |
|------|------|------|--------|
| 2026-03-06 | 0.1 | 초안 작성 | - |
| 2026-03-06 | 0.2 | 교환 제안 뱃지, 끌어올리기 뱃지 추가 | - |
