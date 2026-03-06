# EXCH05 교환 신청함

## 메타 정보
| 속성 | 값 |
|------|-----|
| 화면코드 | EXCH05 |
| 화면명 | 교환 신청함 |
| 라우트 | `/exchange/proposals` (내가 받은 제안) + `/exchange/my-proposals` (내가 보낸 제안) |
| 이용자 | PC / Mobile |
| 버전 | 0.1 |
| 작성일 | 2026-03-06 |
| 상태 | 초안 |

## 화면 개요
> 내 교환글에 들어온 교환 제안(받은 제안)과 내가 다른 글에 보낸 제안(보낸 제안)을 한눈에 관리하는 화면. 제안별 비교, 수락, 거절, 취소를 수행할 수 있다.

## 진입 경로
- 마이페이지 > 교환 신청함
- 교환 상세(EXCH02) > 받은 제안 패널 > 전체보기
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
  - 제안 굿즈: 대표이미지(60px) + 상품명 + 상태등급(S/A/B/C) + 작품·캐릭터
  - 첨부 메시지 (회색 말풍선, 있는 경우만)
  - 제안 시간: "2시간 전" / "3일 전"
  - 상태 뱃지: 대기중(Amber) / 수락됨(Green) / 거절됨(Gray) / 만료(Gray)
  - 액션 버튼 (대기중일 때만):
    - [수락] Blue 버튼
    - [거절] Gray 아웃라인 버튼
    - [채팅] 아이콘 버튼

### ④ 보낸 제안 탭 내용
- 내가 보낸 제안 목록 (최신순)
- 각 제안 카드:
  - 상대방 교환글 요약 (이미지 + 상품명)
  - 내가 제안한 굿즈 (이미지 + 상품명)
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
| groups[].proposals[].offered_exchange | object | 제안 굿즈 {id, title, image, condition, work, character} |
| groups[].proposals[].message | string | 첨부 메시지 (nullable) |
| groups[].proposals[].status | string | pending / accepted / rejected / expired |
| groups[].proposals[].created_at | string | 제안 시간 |

### API: GET /api/exchange/proposals/sent
Response:
| 필드 | 타입 | 설명 |
|------|------|------|
| items | array | 보낸 제안 목록 |
| items[].id | string | 제안 ID |
| items[].target_exchange | object | 상대 교환글 {id, title, image, user} |
| items[].my_exchange | object | 내가 제안한 굿즈 {id, title, image} |
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
| 5 | 정렬 | 받은 제안: 대기중 > 수락됨 > 거절됨/만료 순 |

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

## 연관 화면
| 이동 대상 | 조건 | 화면코드 |
|----------|------|---------|
| 교환 상세 | 교환글/제안 굿즈 탭 | EXCH02 |
| 채팅방 | 채팅 아이콘/수락 후 | CHAT02 |
| 마이페이지 | ← 뒤로가기 | USER01 |

## 개정 이력
| 날짜 | 버전 | 내용 | 작성자 |
|------|------|------|--------|
| 2026-03-06 | 0.1 | 초안 작성 | - |
