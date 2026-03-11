# EXCH05 교환 신청함

## 메타 정보
| 속성 | 값 |
|------|-----|
| 화면코드 | EXCH05 |
| 화면명 | 교환 신청함 |
| 라우트 | `/exchange/proposals` (내가 받은 제안) + `/exchange/my-proposals` (내가 보낸 제안) |
| 이용자 | PC / Mobile |
| 버전 | 0.5 |
| 작성일 | 2026-03-06 |
| 상태 | 초안 |

## 화면 개요
> 내 교환글에 들어온 교환 제안(받은 제안)과 내가 다른 글에 보낸 제안(보낸 제안)을 한눈에 관리하는 화면. 제안별 비교, 수락, 거절, 취소를 수행할 수 있다.

## 진입 경로
- 마이페이지 > 교환 신청함
- 교환 상세(EXCH02) > 받은 제안 패널 > 전체보기
- 교환 제안(EXCH04) > 제안 발송 완료 후 상태 확인
- 알림 > 교환 제안 알림 탭
- 하단 네비게이션 > 마이 > 교환 신청함

## 화면 구성 요소

### ① Header
- 좌측: ← 뒤로가기
- 중앙: "교환 신청함"
- 우측: 없음

### ② 탭 바
- 2개 탭: 받은 제안 (N) | 보낸 제안 (N)
- 탭별 카운트 뱃지 (대기중인 제안 수)

### ③ 받은 제안 탭 내용
- 내 교환글별로 그룹핑
- 각 그룹:
  - 내 교환글 요약 카드 (이미지 + 상품명 + 받은 제안 N건)
  - 해당 글에 대한 제안 리스트 (최신순)

- 각 제안 카드:
  - 제안자 프로필 (32px 아바타 + 닉네임 + 신뢰등급 Lv.N + 평점)
  - 제안 방식: 내 교환글 매칭(`listing`) / 빠른 제안 등록(`manual`)
  - 제안 굿즈: 대표이미지(60px) + 상품명 + 상태등급(S/A/B/C) + 작품·캐릭터
  - 거래 구조: `1:1` / `1:N` / `N:1`
  - 수량 정보: 제공 수량 / 희망 수량
  - 차액 제안: 없음 / `+금액` / `-금액`
  - 오늘 직교환: ON일 때 이벤트명 + 시간대 + 대략 위치 요약 노출
  - 첨부 메시지 (회색 말풍선, 있는 경우만)
  - 제안 시간: "2시간 전" / "3일 전"
- 상태 뱃지: 대기중(Amber) / 수락됨(Green) / 거절됨(Gray) / 취소됨(Gray) / 만료(Gray)
  - 액션 버튼 (대기중일 때만):
    - [수락] Blue 버튼
    - [거절] Gray 아웃라인 버튼
    - [채팅] 아이콘 버튼

### ④ 보낸 제안 탭 내용
- 내가 보낸 제안 목록 (최신순)
- 각 제안 카드:
  - 상대방 교환글 요약 (이미지 + 상품명)
  - 내가 제안한 굿즈 (이미지 + 상품명)
  - 제안 방식 + 거래 구조 + 수량 + 차액 + 오늘 직교환 여부
  - 상태 뱃지: 대기중 / 수락됨 / 거절됨(사유 표시) / 취소됨 / 만료
  - 액션 버튼 (대기중일 때만): [제안 취소]
  - 수락됨일 때: [채팅으로 이동] 버튼

### ⑤ Empty State
- 받은 제안 없음: "아직 받은 교환 제안이 없어요" + 일러스트
- 보낸 제안 없음: "아직 보낸 교환 제안이 없어요" + "교환글 둘러보기" CTA

## 데이터 필드

### API: GET /api/exchange/proposals/received
Response:
| 필드 | 타입 | 설명 |
|------|------|------|
| groups | array | 내 교환글별 그룹 |
| groups[].exchange | object | 내 교환글 요약 {id, title, image, status} |
| groups[].proposals | array | 받은 제안 목록 |
| groups[].proposals[].id | string | 제안 ID |
| groups[].proposals[].proposer | object | 제안자 {id, nickname, avatar, trust_level, rating} |
| groups[].proposals[].proposal_mode | string | `listing` / `manual` |
| groups[].proposals[].offered_exchange | object | 제안 굿즈(매칭 제안) {id, title, image, condition, work, character} (nullable) |
| groups[].proposals[].manual_offer | object | 제안 굿즈(빠른 제안) {images, title, condition, quantity} (nullable) |
| groups[].proposals[].trade_structure | string | `one_to_one` / `one_to_many` / `many_to_one` |
| groups[].proposals[].offer_quantity | number | 제안자 제공 수량 |
| groups[].proposals[].want_quantity | number | 제안자 희망 수량 |
| groups[].proposals[].cash_adjustment | object | {enabled, direction, amount} |
| groups[].proposals[].instant_meetup | object | {enabled, event_name, time_window, meetup_spot} |
| groups[].proposals[].message | string | 첨부 메시지 (nullable) |
| groups[].proposals[].status | string | pending / accepted / rejected / cancelled / expired |
| groups[].proposals[].created_at | string | 제안 시간 |

### API: GET /api/exchange/proposals/sent
Response:
| 필드 | 타입 | 설명 |
|------|------|------|
| items | array | 보낸 제안 목록 |
| items[].id | string | 제안 ID |
| items[].target_exchange | object | 상대 교환글 {id, title, image, user} |
| items[].proposal_mode | string | `listing` / `manual` |
| items[].my_exchange | object | 내가 제안한 굿즈(매칭 제안) {id, title, image} (nullable) |
| items[].manual_offer | object | 내가 제안한 굿즈(빠른 제안) {images, title, condition, quantity} (nullable) |
| items[].trade_structure | string | `one_to_one` / `one_to_many` / `many_to_one` |
| items[].offer_quantity | number | 제공 수량 |
| items[].want_quantity | number | 희망 수량 |
| items[].cash_adjustment | object | {enabled, direction, amount} |
| items[].instant_meetup | object | {enabled, event_name, time_window, meetup_spot} |
| items[].message | string | 첨부 메시지 |
| items[].status | string | pending / accepted / rejected / cancelled / expired |
| items[].reject_reason | string | 거절 사유 (nullable) |
| items[].created_at | string | 제안 시간 |

## 비즈니스 로직
| # | 규칙 | 설명 |
|---|------|------|
| 1 | 제안 자동 만료 | 미응답 7일 경과 시 자동 만료 처리 |
| 2 | 수락 시 일괄 거절 | 하나의 제안 수락 시 같은 교환글의 나머지 대기중 제안 자동 거절 |
| 3 | 제안 취소 | 대기중 상태에서만 취소 가능 |
| 4 | 교환 완료 시 | 교환 완료된 글의 남은 제안 자동 만료 |
| 5 | 정렬 | 받은 제안: 대기중 > 수락됨 > 거절됨/취소됨/만료 순 |
| 6 | 수정 잠금 연동 | 받은 제안이 1건 이상 생성되면 대상 교환글의 `proposal_history_count`가 1 이상이 되어 EXCH02/EXCH03 수정 잠금 조건 충족 |
| 7 | 제안 방식/조건 메타 표시 | EXCH05 카드에서 `proposal_mode`, 거래 구조(1:1/1:N/N:1), 수량, 차액을 요약 노출 |
| 8 | 오늘 직교환 메타 처리 | `instant_meetup.enabled=true`이면 이벤트명/시간대/대략 위치 요약 노출, 시간 만료 후 일반 직거래 제안으로 표시 전환 |
| 9 | 상태 이력 표시 정책 | 받은/보낸 제안 모두 `pending/accepted/rejected/cancelled/expired` 상태를 이력으로 유지 표시 (비pending는 액션 비활성) |

## 인터랙션
| # | 트리거 | 동작 | 비고 |
|---|--------|------|------|
| 1 | 탭 전환 | 받은/보낸 제안 탭 전환 | URL 변경 |
| 2 | 내 교환글 카드 탭 | EXCH02로 이동 | |
| 3 | 제안 굿즈 이미지 탭 | 해당 교환글 상세(EXCH02) 이동 | |
| 4 | [수락] 탭 | 확인 모달 → 수락 처리 → 채팅방 생성 | "수락하시겠습니까?" 확인 |
| 5 | [거절] 탭 | 거절 사유 바텀시트 → 거절 처리 | 사유 선택 (선택사항) |
| 6 | [채팅] 탭 | 채팅방(CHAT02) 이동 | |
| 7 | [제안 취소] 탭 | 확인 모달 → 취소 처리 | "취소하시겠습니까?" |
| 8 | 받은 제안 카드 탭 후 EXCH02에서 [수정] 탭 | `proposal_history_count≥1`이면 수정 잠금 안내 노출 | EXCH03 진입 차단 |
| 9 | 오늘 직교환 이벤트명 탭 | 이벤트 상세 확인 | EVNT02 |

## 연관 화면
| 이동 대상 | 조건 | 화면코드 |
|----------|------|---------|
| 교환 상세 | 교환글/제안 굿즈 탭 | EXCH02 |
| 교환 제안 | 보낸 제안 재작성/재제안 | EXCH04 |
| 교환글 수정 | 본인 글 + 제안 이력 0건에서만 수정 진입 가능 | EXCH03 |
| 수정 잠금 안내 | 본인 글 + 제안 이력 1건 이상에서 수정 시도 | EXCH02 유지 |
| 채팅방 | 채팅 아이콘/수락 후 | CHAT02 |
| 이벤트 상세 | 오늘 직교환 이벤트명 탭 | EVNT02 |
| 마이페이지 | ← 뒤로가기 | USER01 |

## 개정 이력
| 날짜 | 버전 | 내용 | 작성자 |
|------|------|------|--------|
| 2026-03-11 | 0.5 | 받은 제안 상태 모델 보정: `cancelled` 표시 추가, 정렬/상태 이력 표시 정책 정합화 | - |
| 2026-03-11 | 0.4 | EXCH04(교환 제안 전용 페이지) 진입 경로/연관 화면 반영 | - |
| 2026-03-10 | 0.3 | 교환 제안 확장 메타 반영: 제안 방식(매칭/빠른등록), 다수거래(1:1/1:N/N:1), 차액(+/-), 오늘 직교환 정보 표시 스펙 추가 | - |
| 2026-03-10 | 0.2 | 교환 상세/등록 수정 잠금 연동 명세(`proposal_history_count`) 및 인터랙션/연관 화면 보강 | - |
| 2026-03-06 | 0.1 | 초안 작성 | - |
