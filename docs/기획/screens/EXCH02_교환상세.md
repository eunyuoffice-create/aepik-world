# EXCH02 교환 상세

## 메타 정보

| 항목 | 내용 |
|------|------|
| **화면코드** | EXCH02 |
| **화면명** | 교환 상세 |
| **라우트** | /exchange/[id] |
| **이용자** | PC / Mobile |
| **버전** | 0.9 |
| **작성일** | 2026-03-06 |
| **상태** | 확정 |

---

## 화면 개요

> 개별 교환글의 상세 정보를 확인하고, 찜/채팅/교환 제안 등의 액션을 수행하는 화면. 보유 굿즈 사진, 희망 교환 조건, 판매자 신뢰 정보를 종합적으로 제공한다.

---

## 진입 경로

- 교환 목록(EXCH01) > 카드 탭
- 홈(HOME01) > 인기 교환 > 카드 탭
- 검색 결과 > 교환 카드 탭
- 알림 > 교환글 관련 알림 탭
- 채팅방 > 교환글 링크 탭

---

## 화면 구성 요소

### ① Header
- 좌측: ← 뒤로가기
- 우측: 공유 아이콘 + 더보기(⋯) 메뉴
  - 더보기 메뉴: 신고하기 / 차단하기 / URL 복사

### ② 이미지 슬라이더
- 상품 이미지 (최대 5장)
- 좌우 스와이프로 이동
- 인디케이터: 하단 중앙 ● ○ ○ 형태
- 이미지 탭 시: 전체 화면 이미지 뷰어
- 이미지 없는 경우: 회색 플레이스홀더

### ③ 태그 + 상품 정보
- 태그: [거래유형] + [카테고리]
  - 거래유형: 교환 / 교환+차액
  - 카테고리: 애니/만화/소설, 아이돌/아티스트, 배우/무대, 게임
- 상품명 (h1)
- 작품 · 캐릭터 (탭 시 해당 작품 필터 목록 이동)
- 등록일 · 조회수

### ④ 상세 정보 박스
- 상태: S급 (미개봉) — 상태 아이콘 + 텍스트
- 굿즈 타입: 아크릴 스탠드
- 보유 수량: N개
- 희망 교환: OO 캐릭터 OO 굿즈 (상세 텍스트)
- 차액: ±O,OOO원 가능 (또는 "차액 없음")
- 거래 방식: 택배 / 직거래 / 둘 다 가능

### ⑤ 직거래 지도 (조건부)
- 거래 방식이 "직거래" 또는 "둘 다"인 경우에만 표시
- 카카오맵 API 임베드
- 거래 희망 위치 핀 마커
- 주소 텍스트

### ⑥ 추가 조건
- 등록자가 작성한 추가 거래 조건/설명
- 텍스트 영역 (접히기/펼치기 가능 — 3줄 초과 시)

### ⑦ 판매자 정보 박스
- 프로필 이미지 (40px 원형) + 닉네임
- 신뢰 등급 뱃지 (Lv.1~4 + 아이콘)
- ★ 평점 · 거래 횟수
- 최근 활동: "N시간 전 접속"
- 탭 시: 타인 프로필(USER03) 이동

### ⑧ 관련 교환글 (추천)
- 같은 작품/캐릭터의 다른 교환글
- 가로 스크롤 카드 (ProductCard 축소 버전)
- 최대 6개

### ⑨ 하단 고정 CTA
- 좌측: `Heart` 찜 버튼 (토글, 찜 수 표시)
- 중앙: `MessageCircle` 채팅하기 (판매자와 1:1 채팅 시작)
- 우측: `Repeat` 교환 제안 (EXCH04 전용 페이지 이동)
- 비로그인 시: 로그인 유도
- 본인 글일 경우: [수정] [끌어올리기] [삭제] 표시
  - 단, 받은 제안 이력이 1건 이상(상태 무관)이면 [수정] 비활성
  - 안내 문구: "받은 제안 이력이 있어 수정할 수 없어요. 제안이 한 번이라도 도착한 글은 수정할 수 없어요."
  - 받은 제안 이력이 1건 이상이면 [삭제] 대신 [마감] 버튼 표시
  - [마감] 탭 시 확인 팝업: "마감하면 대기중 제안이 모두 만료되며, 다시 활성화할 수 없어요. 마감하시겠습니까?"
- 마감(`closed`) 상태일 경우: 하단 CTA 비활성 + 상단 "마감" 뱃지 표시, 본인도 수정/끌어올리기/삭제 불가

---

## 데이터 필드

### API: GET /api/exchange/[id]

**Response:**

| 필드 | 타입 | 설명 |
|------|------|------|
| id | string | 교환글 ID |
| images | string[] | 이미지 URL 배열 (최대 5) |
| title | string | 상품명 |
| work | object | 작품 {id, name, category} |
| character | object | 캐릭터 {id, name} |
| goods_type | string | 굿즈 타입 |
| condition | string | 상품 상태 (S/A/B/C) |
| quantity | number | 보유 수량 |
| wanted | object | 희망 교환 {work, character, goods_type, description} |
| cash_diff | object | 차액 {enabled, amount, direction(+/-)} |
| trade_method | string | 거래 방법 (delivery/meetup/both) |
| meetup_location | object | 직거래 위치 {lat, lng, address} (nullable) |
| extra_condition | string | 추가 조건 (nullable) |
| user | object | 등록자 {id, nickname, avatar, trust_level, rating, trades, last_active} |
| status | string | 상태 (active/closed/completed/hidden) |
| pending_proposal_count | number | 대기중 제안 수 (본인 글일 때) |
| proposal_history_count | number | 누적 받은 제안 수 (본인 글일 때, 수정 잠금 판단용) |
| can_propose_by_listing | boolean | 내 교환글 매칭 제안 가능 여부 |
| can_propose_by_manual | boolean | 빠른 제안 등록 가능 여부 |
| view_count | number | 조회수 |
| wish_count | number | 찜 수 |
| is_wished | boolean | 현재 사용자 찜 여부 |
| created_at | string | 등록일 |

---

## 비즈니스 로직

| # | 규칙 | 설명 |
|---|------|------|
| 1 | 본인 글 | CTA를 [수정/끌어올리기/삭제]로 변경 |
| 2 | 교환 완료 | CTA 비활성화, 상단 "교환 완료" 뱃지 표시 |
| 3 | 마감 상태 | CTA 비활성화, 상단 "마감" 뱃지 표시, 제안 불가, 찜/공유만 가능 |
| 4 | 비공개 상태 | 본인만 접근 가능, 타인 접근 시 "존재하지 않는 교환글" 표시 |
| 5 | 차단 사용자 | 차단한 사용자의 글 접근 시 "차단된 사용자의 게시글" 표시 |
| 6 | 조회수 | 같은 사용자 하루 1회만 카운트 |
| 7 | 교환 제안 | EXCH04(교환 제안 전용 페이지)로 이동 후 제안 작성 |
| 8 | 교환 제안 제한 | 같은 글에 동시 pending 최대 3건 + 재시도 제한(24시간 3회 / 7일 6회) |
| 9 | 제안 자동 만료 | 미응답 7일 경과 시 자동 만료 |
| 10 | 수락 시 다른 제안 자동 거절 | 하나의 제안 수락 시 나머지 pending 제안은 자동 거절 ('다른 분과 진행합니다') |
| 11 | 수정 잠금 정책 | 본인 글에 받은 제안 이력 1건 이상(상태 무관)이면 수정 불가 + 안내 문구 노출 |
| 12 | 삭제 잠금 + 마감 유도 | 받은 제안 이력 1건 이상이면 삭제 불가, [마감] 버튼으로 종료 유도 |
| 13 | 마감 처리 | 마감 시 pending 제안 일괄 만료 + 제안자 알림 + 재오픈 불가 |
| 14 | 제안 방식 2종 | `listing`(내 교환글 매칭) / `manual`(빠른 제안 등록) 지원 |
| 15 | 빠른 제안 등록 정책 | 빠른 제안은 임시 제안 데이터로 저장되며 공개 교환글 자동 생성 없음 |
| 16 | 다수거래 지원 | 제안 단위에서 `1:1` / `1:N` / `N:1` 및 수량 입력 지원 |
| 17 | 차액 제안 | 제안 단위에서 `+/- 금액` 입력 가능 (최종 확정은 채팅에서 협의) |
| 18 | 오늘 직교환 옵션 | 이벤트 당일 한정으로 시간대+대략 위치(역/랜드마크) 입력 가능, 수락 후 채팅 상단 배너 노출 |
| 19 | 제안 액션 권한 | `accept/reject`는 등록자만, `cancel`은 제안자만 가능 |
| 20 | 제안 상태 전이 | `pending`에서만 상태 변경 허용, `accepted/rejected/cancelled/expired`는 종단 상태 |
| 21 | 동시성 제어 | `accept`는 트랜잭션으로 단일 성공 보장, 동시 요청 실패 시 409 반환 |

---

## 교환 제안 관리 (v0.2 추가)

### 개요

교환 제안 시스템은 다음의 기능을 제공합니다:

1. 다중 사용자의 교환 제안 수신
2. 제안 목록 비교 기능
3. 제안 수락/거절/대기 관리

---

### ⑩ 교환 제안 페이지 진입 (EXCH04)

**동작:**
- 기존 ⑨번의 "교환 제안" 버튼 탭 시 EXCH04로 이동
- EXCH04에서 제안 방식 선택 (`내 교환글 매칭` / `빠른 제안 등록`) 및 조건 입력 후 전송
- 본 화면(EXCH02)에서는 제안 작성 폼을 직접 제공하지 않고 진입 CTA만 제공

---

### ⑪ 받은 제안 패널 (본인 글일 경우)

**표시 조건:**
- 본인 교환글 상세 접속 시 교환 제안 CTA 대신 "받은 제안" 패널 표시 + 하단 본인 액션 CTA([수정/끌어올리기/삭제]) 노출
- 받은 제안 이력이 1건 이상(상태 무관)이면 하단 본인 CTA의 [수정] 비활성 상태 유지

**구성 요소:**
- 제안 카운트 뱃지: "받은 제안 3건"
- 탭 시 받은 제안 목록 확장 (또는 EXCH05 이동)

**각 제안 카드 구성:**
- 제안자 프로필 (아바타 + 닉네임 + 신뢰등급 + 평점)
- 제안 굿즈 이미지 + 상품명 + 상태등급
- 첨부 메시지 (있는 경우)
- 제안 시간 (N시간 전)
- 액션 버튼: [수락] [거절] [채팅]

**수락 플로우:**
- 수락 시: 확인 모달 → 양쪽 알림 → 채팅방 자동 생성 → 교환 진행 단계 시작

**거절 플로우:**
- 거절 시: 사유 선택 바텀시트 (선택사항)
  - 사유 예시칩: "다른 분과 진행합니다" / "조건이 맞지 않습니다" / "상태가 기대와 다릅니다"
  - 거절 알림 → 제안자에게 발송

---

### 교환 제안 상태

| 상태 | 설명 | 제안자 화면 | 등록자 화면 |
|------|------|------------|------------|
| pending | 제안 대기 | "제안 중" 뱃지 | 받은 제안 목록에 표시 |
| accepted | 수락됨 | "수락됨" + 채팅 이동 | 교환 진행 시작 |
| rejected | 거절됨 | "거절됨" + 사유(있는 경우) | 받은 제안 목록에 상태 배지로 표시(액션 비활성) |
| cancelled | 제안 취소 | 취소 완료 | 받은 제안 목록에 상태 배지로 표시(액션 비활성) |
| expired | 만료 | 자동 만료 (7일) | 받은 제안 목록에 상태 배지로 표시(액션 비활성) |

---

### API: POST /api/exchange/[id]/proposal

**설명:** 교환 제안 생성

**Request Body:**

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| proposal_mode | string | O | `listing` / `manual` |
| my_exchange_id | string | 조건부 | 내 교환글 ID (`proposal_mode=listing`) |
| manual_offer.images | string[] | 조건부 | 임시 제안 이미지 (최대 3장, `proposal_mode=manual`) |
| manual_offer.title | string | 조건부 | 임시 제안 굿즈명 |
| manual_offer.condition | string | 조건부 | S / A / B / C |
| manual_offer.quantity | number | 조건부 | 임시 제안 수량 |
| trade_structure | string | O | `one_to_one` / `one_to_many` / `many_to_one` |
| offer_quantity | number | O | 제안자가 제공할 수량 |
| want_quantity | number | O | 제안자가 희망하는 수량 |
| cash_adjustment | object | X | {enabled, direction(+/-), amount} |
| instant_meetup | object | X | {enabled, event_name, time_window, meetup_spot} |
| message | string | X | 첨부 메시지 (100자) |

---

### API: PATCH /api/exchange/[id]/proposal/[proposalId]

**설명:** 교환 제안 상태 변경 (수락/거절/취소)

**Request Body:**

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| action | string | O | accept / reject / cancel |
| reject_reason | string | X | 거절 사유 |

**서버 검증 규칙:**
- `action=accept/reject`: 등록자(owner)만 가능
- `action=cancel`: 제안자(proposer)만 가능
- 현재 상태가 `pending`일 때만 처리
- `accept` 동시 요청 시 최초 1건만 성공, 나머지는 충돌(`409`) 처리

---

### API: GET /api/exchange/[id]/proposals

**설명:** 받은 제안 목록 조회 (등록자만 접근 가능)

**Response:** 제안 목록 배열

| 필드 | 타입 | 설명 |
|------|------|------|
| id | string | 제안 ID |
| proposer | object | 제안자 {id, nickname, avatar, trust_level, rating} |
| proposal_mode | string | `listing` / `manual` |
| offered_exchange | object | 제안할 굿즈(매칭 제안) {id, title, image, condition, work, character} (nullable) |
| manual_offer | object | 임시 제안 굿즈 {images, title, condition, quantity} (nullable) |
| trade_structure | string | `one_to_one` / `one_to_many` / `many_to_one` |
| offer_quantity | number | 제안자 제공 수량 |
| want_quantity | number | 제안자 희망 수량 |
| cash_adjustment | object | {enabled, direction, amount} |
| instant_meetup | object | {enabled, event_name, time_window, meetup_spot} |
| message | string | 첨부 메시지 |
| status | string | pending / accepted / rejected / cancelled / expired |
| reject_reason | string | 거절 사유 (nullable) |
| created_at | string | 제안 시간 |

---

## 인터랙션

| # | 트리거 | 동작 | 비고 |
|---|--------|------|------|
| 1 | ← 탭 | 이전 페이지 이동 | |
| 2 | 이미지 스와이프 | 다음/이전 이미지 | 인디케이터 업데이트 |
| 3 | 이미지 탭 | 전체 화면 이미지 뷰어 | 핀치 줌 지원 |
| 4 | 작품명 탭 | 해당 작품 필터 목록(EXCH01) | work_id 파라미터 |
| 5 | `Heart` 탭 | 찜 토글 | 즉시 반영, 카운트 업데이트 |
| 6 | `MessageCircle` 탭 | 채팅방 생성/이동 | CHAT02 |
| 7 | `Repeat` 탭 | EXCH04(교환 제안 페이지)로 이동 | 대상 글 id 전달 |
| 8 | EXCH04에서 제안 발송 완료 후 복귀 | 완료 토스트 + EXCH05 반영 확인 | 선택 |
| 9 | 판매자 영역 탭 | 타인 프로필 이동 | USER03 |
| 10 | 공유 탭 | 공유 시트 (카카오톡/URL 복사/트위터) | |
| 11 | ⋯ > 신고 | 신고 바텀시트 | 사유 선택 |
| 12 | 받은 제안 탭 | 제안 목록 확장 or EXCH05 이동 | 본인 글만 |
| 13 | 제안 수락 (등록자 + pending) | 확인 모달 → 채팅방 생성 → 진행 시작 | 알림 발송 |
| 14 | 제안 거절 (등록자 + pending) | 사유 선택 → 제안자 알림 | 선택사항 |
| 15 | 제안 취소 (제안자 + pending) | 취소 처리 후 상태 갱신 | EXCH05 보낸 제안 연동 |
| 16 | 본인 글 `수정` 탭 (proposal_history_count≥1) | 수정 불가 안내 문구/토스트 노출 | EXCH03 진입 차단 |

---

## 연관 화면

| 이동 대상 | 조건 | 화면코드 |
|----------|------|---------|
| 교환 목록 | ← 뒤로가기 | EXCH01 |
| 교환글 등록(수정) | 본인 글 수정 (받은 제안 이력 0건) | EXCH03 |
| 교환 제안 작성 | `Repeat` 탭 | EXCH04 |
| 채팅방 | `MessageCircle` 탭 | CHAT02 |
| 타인 프로필 | 판매자 영역 탭 | USER03 |
| 이미지 뷰어 | 이미지 탭 | 모달 |
| 로그인 | 비로그인 + CTA | AUTH01 |
| 교환 신청함 | 받은 제안 탭 | EXCH05 |
| 이벤트 상세 | 오늘 직교환 옵션에서 이벤트 확인 | EVNT02 |

---

## 개정 이력

| 날짜 | 버전 | 내용 | 작성자 |
|------|------|------|--------|
| 2026-03-11 | 1.0 | 마감(closed) 상태 추가: CTA/뱃지/비즈니스 로직/삭제 잠금 대체 정의 | - |
| 2026-03-11 | 0.9 | 제안 상태 전이/권한 검증 명세 추가, 동시성 제어(accept 단일 성공) 및 상태 표시 정책(취소/만료 포함) 정합화 | - |
| 2026-03-11 | 0.8 | 교환 제안 전용 화면(EXCH04) 진입 구조로 변경: EXCH02는 CTA 진입만 담당하도록 정합화 | - |
| 2026-03-10 | 0.7 | 교환 제안 확장: 내 글 매칭/빠른 제안 등록 2종, 다수거래(1:N/N:1), 차액 제안, 오늘 직교환(이벤트 근처) 옵션 추가 | - |
| 2026-03-10 | 0.6 | COMM01/COMM02 공통 디자인 토큰/컴포넌트 기준으로 상세 화면 스타일 정합화 반영 | - |
| 2026-03-10 | 0.5 | 본인 글 상세에서 받은 제안 패널 + 본인 CTA 동시 노출 구조로 문구 정합성 보정 | - |
| 2026-03-10 | 0.4 | 수정 잠금 기준을 pending에서 제안 이력 전체(상태 무관)로 강화 | - |
| 2026-03-10 | 0.3 | 받은 제안 이력 기반 수정 잠금 정책(초안) 추가 | - |
| 2026-03-06 | 0.2 | 교환 제안 관리 시스템 추가 (⑩⑪ 섹션, API 스펙, 상태 관리) | - |
| 2026-03-06 | 0.1 | 초안 작성 | - |
