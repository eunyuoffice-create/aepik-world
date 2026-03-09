# 애픽 월드 (Aepik World) — 프로젝트 작업 규칙

> 이 파일은 Claude가 새 세션에서도 일관된 작업을 수행하기 위한 프로젝트 규칙입니다.
> 작업 시작 전 반드시 이 파일을 읽어주세요.

---

## 1. 프로젝트 개요

- **서비스명**: 애픽 월드 (Aepik World) — 팬굿즈 교환 & 공동구매 플랫폼
- **개발 형태**: 1인 개발 (솔로 프로젝트)
- **4대 카테고리**: 애니/만화/소설(Purple #8B5CF6), 아이돌/아티스트(Pink #EC4899), 배우/무대(Amber #F59E0B), 게임(Cyan #06B6D4)
- **API enum**: `anime_manga_novel / idol_artist / actor_stage / game`
- **기술 스택**: Next.js 15, NestJS, PostgreSQL(Railway), Prisma, Firebase Realtime DB, FCM, AWS S3+CloudFront, Sharp, PortOne, 카카오맵 API, JWT+Upstash Redis, Zod, Sentry, Docker, GitHub Actions

---

## 2. 폴더 구조

```
/OD/
├── CLAUDE.md                     ← 이 파일 (프로젝트 규칙)
├── docs/
│   ├── 애픽월드_Aepik_World_PRD_v5.md    ← 메인 PRD (최신)
│   ├── 애픽월드_디자인_컨셉가이드.md       ← 디자인 시스템
│   ├── 애픽월드_Admin_PRD_v1.md           ← 어드민 PRD
│   ├── 애픽월드_이용약관_초안.md
│   ├── 애픽월드_개인정보처리방침_초안.md
│   ├── 기획/
│   │   ├── 애픽월드_IA_정보구조.md         ← IA 마크다운 원본
│   │   ├── 애픽월드_유저플로우.md           ← 플로우 마크다운 원본
│   │   ├── 애픽월드_화면기획_템플릿_가이드.md
│   │   └── screens/                       ← 화면별 기획서 (.md)
│   │       ├── EXCH01_교환목록.md
│   │       ├── EXCH02_교환상세.md
│   │       ├── EXCH03_교환등록.md
│   │       ├── EXCH05_교환신청함.md
│   │       ├── GRPB01_공구목록.md
│   │       ├── GRPB02_공구상세.md
│   │       ├── GRPB03_공구등록.md
│   │       ├── GRPB04_공구관리.md
│   │       ├── COMM01_디자인토큰.md          ← 공통 디자인 토큰 기획서
│   │       ├── COMM02_공통컴포넌트.md         ← 공통 UI 컴포넌트 기획서
│   │       ├── USER01_마이페이지.md
│   │       ├── USER02_내활동관리.md
│   │       ├── USER03_설정.md
│   │       ├── CHAT01_채팅목록.md
│   │       ├── CHAT02_채팅대화.md
│   │       ├── WANT01_구함.md
│   │       ├── EVNT01_이벤트캘린더.md
│   │       ├── EVNT02_이벤트상세.md
│   │       ├── EVNT03_이벤트등록.md
│   │       ├── BANR01_배너팝업신청.md
│   │       ├── NOTI01_알림센터.md
│   │       └── STAT01_정적페이지.md
│   └── 이전버전/                           ← PRD v3, v4 등 아카이브
├── wireframe/                             ← 와이어프레임 HTML
│   ├── WBS_프로젝트현황.html               ← WBS (와이어프레임 링크 포함)
│   ├── IA_사이트맵.html                    ← IA 비주얼 사이트맵
│   ├── FLOW_전체플로우.html                ← 유저 플로우 다이어그램
│   ├── COMM01_디자인토큰.html              ← 디자인 토큰 레퍼런스
│   ├── COMM02_공통컴포넌트.html            ← 공통 UI 컴포넌트 가이드
│   ├── HOME01_메인홈.html
│   ├── HOME01_디자인시안.html
│   ├── EXCH01_교환목록.html
│   ├── EXCH02_교환상세.html
│   ├── EXCH03_교환등록.html
│   ├── EXCH05_교환신청함.html
│   ├── GRPB01_공구목록.html
│   ├── GRPB02_공구상세.html
│   ├── GRPB03_공구등록.html
│   ├── GRPB04_공구관리.html
│   ├── USER01_마이페이지.html
│   ├── USER02_내활동관리.html
│   ├── USER03_설정.html
│   ├── CHAT01_채팅목록.html
│   ├── CHAT02_채팅대화.html
│   ├── WANT01_구함.html
│   ├── EVNT01_이벤트캘린더.html
│   ├── EVNT02_이벤트상세.html
│   ├── EVNT03_이벤트등록.html
│   ├── BANR01_배너팝업신청.html
│   ├── NOTI01_알림센터.html
│   └── STAT01_정적페이지.html
└── images/
    └── 로고.png
```

---

## 3. ⚠️ 연쇄 업데이트 규칙 (가장 중요!)

**기능 추가/변경/삭제 시 반드시 아래 문서들을 연쇄적으로 업데이트해야 합니다.**

### 업데이트 순서

| 순서 | 문서 | 경로 | 업데이트 내용 |
|------|------|------|---------------|
| 1 | **PRD** | `docs/애픽월드_Aepik_World_PRD_v5.md` | 기능 정의, 비즈니스 로직, API 스펙 |
| 2 | **화면 기획서** | `docs/기획/screens/XXXX_화면명.md` | 해당 화면의 상세 기획 (신규면 생성) |
| 3 | **IA 사이트맵 (MD)** | `docs/기획/애픽월드_IA_정보구조.md` | 페이지 구조, 라우트 |
| 4 | **IA 사이트맵 (HTML)** | `wireframe/IA_사이트맵.html` | 비주얼 사이트맵 반영 |
| 5 | **유저 플로우 (MD)** | `docs/기획/애픽월드_유저플로우.md` | 플로우 텍스트 원본 |
| 6 | **유저 플로우 (HTML)** | `wireframe/FLOW_전체플로우.html` | 비주얼 플로우 다이어그램 반영 |
| 7 | **와이어프레임 (HTML)** | `wireframe/XXXX_화면명.html` | 해당 화면 와이어프레임 (신규면 생성) |
| 8 | **WBS** | `wireframe/WBS_프로젝트현황.html` | 진행 상태, 와이어프레임 링크 |
| 9 | **디자인 가이드** | `docs/애픽월드_디자인_컨셉가이드.md` | UI 패턴이 변경된 경우만 |
| 10 | **COMM01 디자인 토큰** | `docs/기획/screens/COMM01_디자인토큰.md` + `wireframe/COMM01_디자인토큰.html` | 컬러/폰트/간격 등 토큰 변경 시 |
| 11 | **COMM02 공통 컴포넌트** | `docs/기획/screens/COMM02_공통컴포넌트.md` + `wireframe/COMM02_공통컴포넌트.html` | 공통 컴포넌트 스펙 변경 시 |
| 12 | **어드민 PRD** | `docs/애픽월드_Admin_PRD_v1.md` | 프론트 기능 변경이 어드민에 영향 줄 때 |

### 영향 범위별 체크리스트

**새 화면 추가 시:**
- [ ] PRD에 관련 섹션 추가/수정
- [ ] 화면 기획서 MD 생성 (`docs/기획/screens/`)
- [ ] IA 사이트맵 MD + HTML 업데이트
- [ ] 유저 플로우 MD + HTML 업데이트
- [ ] 와이어프레임 HTML 생성 (`wireframe/`)
- [ ] WBS에 항목 추가 + 와이어프레임 링크 연결
- [ ] COMM02에 없는 새 공통 컴포넌트가 필요하면 COMM02에 먼저 추가
- [ ] 어드민 PRD 영향 확인 → 어드민에서 관리해야 할 데이터/상태가 추가되었는지 체크

**기존 기능 변경 시:**
- [ ] PRD 해당 섹션 수정
- [ ] 화면 기획서 MD 수정
- [ ] IA 사이트맵 변경 여부 확인 (라우트 변경 시)
- [ ] 유저 플로우 변경 여부 확인 (흐름 변경 시)
- [ ] 와이어프레임 HTML 수정
- [ ] WBS 상태 업데이트
- [ ] 공통 컴포넌트 스펙 변경 영향 확인 → COMM01/COMM02 업데이트 필요 여부 체크
- [ ] 어드민 PRD 영향 확인 → 상태/로직/데이터 변경이 어드민 관리 기능에 영향 주는지 체크

**비즈니스 로직만 변경 시 (UI 변경 없음):**
- [ ] PRD 해당 섹션 수정
- [ ] 화면 기획서 비즈니스 로직 테이블 수정
- [ ] 어드민 PRD 영향 확인 → 비즈니스 로직 변경이 어드민 처리/조회에 영향 주는지 체크

### ⚠️ 와이어프레임 ↔ 기획서 연동 규칙

**기획서(MD)에 UI 요소가 추가/변경되면, 반드시 해당 와이어프레임(HTML)의 폰 프레임에도 반영해야 합니다.**

예시:
- 기획서에 "교환 제안 바텀시트" UI가 추가됨 → 와이어프레임에 해당 바텀시트 phone frame 추가 필요
- 기획서에 "받은 제안 패널" UI가 추가됨 → 와이어프레임에 해당 패널 phone frame 추가 필요
- 기획서에 상태 뱃지(진행 결정 대기, 무산 등)가 추가됨 → 목록 와이어프레임 카드에도 뱃지 반영

**연관 화면도 확인:**
- 상세(EXCH02/GRPB02) 기능 변경 → 목록(EXCH01/GRPB01) 카드에 뱃지/표시 추가 필요한지 확인
- 새로운 상태(진행 결정 대기, 무산, 제안 중 등) 추가 → 해당 상태가 표시되는 모든 화면의 기획서 + 와이어프레임 업데이트
- 새로운 플로우(교환 제안, 끌어올리기 등) 추가 → 해당 기능의 진입점이 되는 화면에 CTA/버튼 반영

**검증 체크리스트:**
- [ ] 기획서 MD의 모든 UI 요소가 와이어프레임 HTML에 phone frame으로 존재하는가?
- [ ] 기획서 MD의 상태 테이블에 있는 모든 상태가 와이어프레임에 시각적으로 표현되었는가?
- [ ] 연관 화면(목록 ↔ 상세, 등록 → 목록 등)에 미치는 영향이 반영되었는가?
- [ ] COMM01/COMM02에 정의된 컴포넌트를 사용했는가? 새 패턴이면 COMM02에 먼저 등록했는가?

### ⚠️ 공통 디자인 시스템(COMM01/COMM02) 양방향 연동 규칙

**COMM01/COMM02는 모든 화면의 기반이므로, 아래 두 방향 모두에서 연쇄 업데이트가 필요합니다.**

**방향 1: 공통 → 개별 화면 (기획/와이어프레임/개발 시 참고)**
- 새 화면 기획 시: COMM01(토큰) + COMM02(컴포넌트) MD를 먼저 읽고, 정의된 스펙을 따를 것
- 와이어프레임 작성 시: COMM02의 컴포넌트 스타일(크기, 색상, 라운딩, 상태)을 그대로 사용할 것
- 개발 시: COMM01의 디자인 토큰 값(컬러 코드, 폰트 크기, 간격, 그림자)을 CSS 변수/상수로 사용할 것
- 개발 시: COMM02의 컴포넌트를 재사용 가능한 공통 컴포넌트로 구현할 것 (버튼, 인풋, 카드, 바텀시트 등)

**방향 2: 개별 화면 → 공통 (기획/개발 개선 시 공통도 맞춰 수정)**
- 개별 화면에서 더 나은 UI 패턴을 발견하면 → COMM02 MD + HTML에 반영 (신규 등록 또는 기존 스펙 수정)
- 개발 중 컴포넌트 스펙 개선(접근성, 성능, UX)이 있으면 → COMM02에 업데이트하고 다른 화면에도 일괄 적용
- 디자인 토큰 변경(컬러, 폰트 등)이 필요하면 → COMM01에 먼저 반영 후 관련 화면 일괄 수정
- PRD에서 비즈니스 로직 변경으로 UI 상태가 추가/삭제되면 → COMM02의 해당 컴포넌트 상태 테이블도 업데이트

**COMM 변경 시 영향 범위 체크:**
- [ ] COMM01 토큰 변경 → 해당 토큰을 사용하는 모든 화면 와이어프레임 + 기획서 확인
- [ ] COMM02 컴포넌트 스펙 변경 → 해당 컴포넌트를 사용하는 모든 화면 와이어프레임 + 기획서 확인
- [ ] 변경된 COMM MD와 HTML이 서로 일치하는가?

### ⚠️ 어드민 PRD 연동 규칙

**프론트(서비스) 기능이 변경되면, 어드민에도 영향이 있는지 반드시 확인해야 합니다.**

**프론트 → 어드민 연쇄 체크 항목:**

| 프론트 변경 | 어드민 확인 사항 |
|-------------|------------------|
| 새 데이터 모델/상태 추가 | 어드민에서 조회/관리 필요 여부 → 매물 관리, 회원 관리 등 해당 섹션 업데이트 |
| 신고/제재 관련 변경 | 어드민 8번(신고 & 제재 관리) 섹션 업데이트 |
| 거래 상태 흐름 변경 | 어드민 6번(매물 관리), 7번(공구 관리) 상태 필터/액션 업데이트 |
| 결제/정산 변경 | 어드민 12번(통계) 또는 향후 정산 섹션 업데이트 |
| 알림 타입 추가 | 어드민 11번(알림 & 공지 관리) 업데이트 |
| 작품/캐릭터 구조 변경 | 어드민 9번(작품/캐릭터 관리) 업데이트 |
| 이벤트/배너/팝업 변경 | 어드민에 이벤트 관리 기능 반영 필요 여부 확인 |
| 새 Phase 기능 추가 | 어드민 14번(Phase별 개발 범위) 업데이트 |

**어드민 PRD 영향 판단 기준:**
- 어드민에서 **조회**해야 하는 데이터가 바뀌었는가?
- 어드민에서 **승인/거절/제재** 등 액션을 취해야 하는 항목이 바뀌었는가?
- 어드민 **대시보드 지표**에 영향을 주는 변경인가?
- 어드민 **API**에 영향이 있는가? (admin guard가 붙는 API 추가/변경)

---

## 4. 화면 ID 네이밍 규칙

| 접두사 | 의미 | 예시 |
|--------|------|------|
| HOME | 메인 홈 | HOME01 |
| EXCH | 교환 | EXCH01~05 |
| GRPB | 공동구매 | GRPB01~04 |
| WANT | 구함 | WANT01 |
| AUTH | 인증 | AUTH01~02 |
| USER | 유저/마이페이지 | USER01~03 |
| CHAT | 채팅 | CHAT01~02 |
| SRCH | 검색 | SRCH01 |
| NOTI | 알림 | NOTI01 |
| EVNT | 이벤트 | EVNT01~03 |
| BANR | 배너/팝업 | BANR01 |
| STAT | 정적 페이지 | STAT01 |
| COMM | 공통 디자인 시스템 | COMM01~02 |

---

## 5. 버전 관리 규칙

- PRD: 메이저 변경 시 `v5` → `v6`, 마이너는 섹션 내 `[v5 추가]` 태그
- 화면 기획서: `v0.1` → `v0.2` (하단 리비전 히스토리 테이블 기록)
- IA/플로우 HTML: `v1.0` → `v1.1` (header meta + footer 텍스트)
- WBS: 별도 버전 없음, 상태만 업데이트

---

## 6. 와이어프레임 HTML 작성 규칙

> **레퍼런스 파일: `wireframe/HOME01_메인홈.html`**
> 모든 와이어프레임 HTML은 HOME01의 레이아웃·CSS·디스크립션 구조를 **그대로** 따라야 합니다.

### 6-1. 전체 구조 (필수)

| 영역 | 클래스 | 스타일 |
|------|--------|--------|
| **메타 헤더** | `.meta-bar` | 흰 배경, `border-bottom: 2px solid #2563EB`, padding 12px 24px |
| **메인 레이아웃** | `.main-layout` | `display: flex; min-height: calc(100vh - 48px)` |
| **와이어프레임 영역** | `.wireframe-side` | `flex: 0 0 55%; background: #EAEAEA; padding: 32px; justify-content: center` |
| **디스크립션 영역** | `.desc-side` | `flex: 0 0 45%; background: #fff; padding: 24px 28px; border-left: 1px solid #ddd` |
| **리비전 푸터** | `.revision-bar` | 흰 배경, `border-top: 1px solid #eee`, table 형태 (날짜/버전/내용/작성자) |

### 6-2. 메타 헤더 (.meta-bar)

```html
<div class="meta-bar">
  <span class="title">XXXX</span>  <!-- 화면코드 -->
  <span class="meta-item"><span class="meta-label">화면명</span><span class="meta-value">화면명</span></span>
  <span class="meta-item"><span class="meta-label">라우트</span><span class="meta-value">/route</span></span>
  <span class="meta-item"><span class="meta-label">이용자</span><span class="meta-value">Mobile / PC</span></span>
  <span class="meta-item"><span class="meta-label">버전</span><span class="meta-value">0.1</span></span>
  <span class="meta-item"><span class="meta-label">상태</span><span class="meta-value">초안</span></span>
  <span class="meta-item"><span class="meta-label">작성일</span><span class="meta-value">2026-03-06</span></span>
</div>
```

### 6-3. 폰 프레임

- 너비: **375px**, 흰 배경, `border-radius: 16px`, `box-shadow: 0 4px 24px rgba(0,0,0,0.12)`
- **두꺼운 테두리(border: 8px solid #333) 금지**, 노치 시뮬레이션(`::before`) 금지
- 다중 프레임 필요 시: `.phone-stack` (`display: flex; flex-direction: column; gap: 24px`) 안에 여러 `.phone-frame`을 세로 배치
- 각 프레임 위에 `<div class="frame-label">프레임명</div>` 라벨 추가 가능

### 6-4. 넘버 서클 (와이어프레임 위 번호 뱃지)

```css
.num-circle {
  position: absolute; z-index: 10;
  background: #2563EB; color: #fff; width: 20px; height: 20px;
  border-radius: 50%; display: flex; align-items: center; justify-content: center;
  font-size: 10px; font-weight: 700; box-shadow: 0 1px 4px rgba(0,0,0,0.2);
}
```

- 와이어프레임 각 섹션(`.wf-section`)의 좌상단에 절대 위치로 배치
- 번호는 디스크립션 패널의 번호와 1:1 대응

### 6-5. 디스크립션 패널 (우측) — 반드시 플랫 넘버드 리스트

> **탭(Tab) 구조 금지** — 모든 항목을 평면 번호 리스트로 나열합니다.

각 항목의 구조:
```html
<div class="desc-item">
  <div class="desc-num-row">
    <div class="desc-num">1</div>                    <!-- 22px 파란 원 -->
    <span class="desc-comp-type">[Header]</span>      <!-- 10px 회색 배경 태그 -->
    <span class="desc-comp-name">앱 헤더</span>       <!-- 14px 600 weight -->
  </div>
  <div class="desc-body">
    <ul>
      <li>설명 항목 1</li>
      <li>설명 항목 2
        <ul class="sub">
          <li>하위 항목 (└ 접두사)</li>
        </ul>
      </li>
    </ul>
  </div>
</div>
<hr class="desc-divider">  <!-- 점선 구분선: border-top: 1px dashed #eee -->
```

- `desc-body`: `padding-left: 30px; font-size: 12px; color: #555; line-height: 1.7`
- 불릿: `•` (파란색 #2563EB), 서브 불릿: `└` (회색 #ccc)

### 6-6. 디스크립션 하단 필수 섹션

번호 항목 이후 반드시 아래 2개 섹션을 포함:

1. **화면 상태** — 로딩/빈 상태/에러/비로그인 등
2. **연관 화면** — 테이블 (이동 대상 / 조건 / 화면코드) 3열

### 6-7. 리비전 푸터

```html
<div class="revision-bar">
  <table>
    <thead><tr><th>날짜</th><th>버전</th><th>내용</th><th>작성자</th></tr></thead>
    <tbody><tr><td>2026-03-06</td><td>0.1</td><td>초안 작성</td><td>-</td></tr></tbody>
  </table>
</div>
```

### 6-8. 반응형

```css
@media (max-width: 900px) {
  .main-layout { flex-direction: column; }
  .wireframe-side { flex: none; padding: 16px; }
  .desc-side { flex: none; border-left: none; border-top: 1px solid #ddd; }
}
```

### 6-9. 기타 규칙

- **CSS 기반**: HOME01의 CSS 블록을 그대로 복사한 뒤, 화면별 와이어프레임 요소 스타일만 추가
- **카테고리 컬러**: Purple(#8B5CF6)/Pink(#EC4899)/Amber(#F59E0B)/Cyan(#06B6D4)
- **WBS 링크**: 새 와이어프레임 생성 시 WBS의 산출물 열에 `<a class="wire-link" target="_blank" href="파일명.html">` 추가. **반드시 `target="_blank"`를 포함하여 새 창(탭)에서 열리도록 할 것.**
- **WBS 진척률 컬러**: 진척률 100% 항목의 `.fill` div에는 반드시 `style="width:100%;background:var(--green)"` 형식으로 초록색 배경을 적용할 것. `background:var(--green)` 누락 금지.
- **이모지 사용 원칙**: 모든 산출물에서 이모지(emoji) 대신 Lucide 스타일 인라인 SVG 아이콘을 사용한다.
  - **HTML 파일** (와이어프레임, 디자인시안, IA, FLOW, WBS 등): 반드시 인라인 SVG 사용 (`viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"`). HOME01_디자인시안.html의 아이콘 패턴 참고
  - **Markdown 파일** (기획서, PRD, 디자인가이드 등): 백틱 감싼 Lucide 아이콘명 사용 (예: `Home`, `Search`, `ArrowRight`). 체크/비체크는 O / X 텍스트 사용
  - **CSS content 속성**: SVG를 직접 삽입할 수 없으므로 `data:image/svg+xml` URL 인코딩 형식 사용
  - **이모지 허용 예외**: SVG를 삽입할 수 없는 환경(외부 플랫폼 게시글, 순수 텍스트 채널 등)에서만 이모지 허용. 프로젝트 내부 산출물에서는 금지
- **독립 실행**: 단일 HTML 파일, 외부 의존성 없음

### 공통 디자인 시스템 참고 필수

**새 화면 와이어프레임 또는 기획서 작성 시, 반드시 아래 공통 문서를 먼저 읽고 준수해야 합니다.**

| 문서 | 경로 | 참고 내용 |
|------|------|-----------|
| **COMM01 디자인 토큰** | `wireframe/COMM01_디자인토큰.html` / `docs/기획/screens/COMM01_디자인토큰.md` | 컬러 팔레트, 타이포그래피, 스페이싱, 그림자, 반응형 브레이크포인트, 모션 |
| **COMM02 공통 컴포넌트** | `wireframe/COMM02_공통컴포넌트.html` / `docs/기획/screens/COMM02_공통컴포넌트.md` | 버튼, 인풋, 태그/뱃지, 팝업/오버레이, 카드, 네비게이션, 검색(필터 바텀시트 포함), 이미지 업로드, 캐러셀, 프로필/신뢰도, 스텝퍼, 알림, 채팅, 지도, 캘린더, 카운트 뱃지, 아바타 |

- **기획 시**: COMM01(토큰) + COMM02(컴포넌트)를 먼저 읽고, 정의된 스펙을 따를 것
- **와이어프레임 시**: COMM02의 컴포넌트 스타일을 그대로 사용하고, 새 패턴이면 COMM02에 먼저 등록
- **개발 시**: COMM01 토큰을 CSS 변수로, COMM02 컴포넌트를 재사용 가능한 공통 컴포넌트로 구현
- **개선 시**: 더 나은 방향으로 수정되면 반드시 COMM01/COMM02도 함께 업데이트 (3번 양방향 연동 규칙 참조)

---

## 7. 핵심 비즈니스 로직 요약

### 교환 제안 시스템
- 1인당 동일 글에 최대 3건 제안
- 5개 상태: pending / accepted / rejected / cancelled / expired
- 수락 시 나머지 자동 거절, 7일 미응답 시 자동 만료

### 공구 상태 흐름
- 모집중 → 진행 결정 대기 → 진행 확정/무산 → 배송중 → 완료
- 자동 무산 타이머: D+0 대기진입, D+1 리마인더, D+3 경고, D+5 자동 무산

### 이벤트 배너 & 팝업 (별도 상품)
- 배너: 메인 캐러셀 max 8슬롯/일, 피드 삽입 max 4슬롯/일
- 팝업: max 3건/일, 1일 1회/팝업, "오늘 하루 안 보기", 순차 노출
- 배너와 팝업은 별도 신청 (둘 다 / 하나만 가능)
- 용도: 팬 이벤트(생카, 데뷔 기념 등) 알림 — 상품 광고 아님

### 결제 시스템
- PortOne (카카오페이, 토스페이, 카드)
- 끌어올리기: 1일 1회 무료, 추가 500~1,000원/회
- Phase 1 무료 → Phase 1.5 유료 전환

### 구함 기반 매칭 알림
- Phase 1: 작품+캐릭터 수준 느슨한 매칭
- Phase 2: 버전 수준 정밀 매칭

---

## 8. 작업 시 주의사항

1. **한국어 사용**: 모든 기획 문서, 기획서, 와이어프레임 내 텍스트는 한국어
2. **파일명**: 한국어 포함 가능 (`EXCH01_교환목록.html`)
3. **PRD 편집 시**: 기존 섹션 번호를 깨트리지 않도록 주의
4. **와이어프레임 HTML**: 독립 실행 가능한 단일 HTML 파일 (외부 의존성 없음)
5. **WBS 링크**: 와이어프레임 HTML이 존재하는 항목만 링크 추가 (미완성은 텍스트만)
6. **경쟁사 참고**: 테이크미(TMM)·윗치폼(Witchform)은 판매 플랫폼, 애픽 월드는 교환 중심 — 포지셔닝이 다름

---

## 9. 현재 진행 상태 (2026-03-07 기준)

### 완료된 항목
- PRD v5 (교환 제안, 매칭 알림, 이벤트 배너&팝업, 결제 시스템 포함)
- 디자인 컨셉가이드 (반응형 브레이크포인트 정책 포함)
- 화면 기획서: AUTH01~02, EXCH01~03, EXCH05, GRPB01~04, COMM01~02, SRCH01, USER01~03, CHAT01~02, WANT01, EVNT01~03, BANR01, NOTI01, STAT01
- 와이어프레임: HOME01, AUTH01~02, EXCH01~03, EXCH05, GRPB01~04, COMM01~02, SRCH01, USER01~03, CHAT01~02, WANT01, EVNT01~03, BANR01, NOTI01, STAT01
- IA 사이트맵 v1.4, 유저 플로우 v1.4
- WBS (전 화면 기획/와이어프레임 완료, 와이어프레임 링크 연결 완료)
- 공통 디자인 시스템: COMM01(디자인 토큰 레퍼런스), COMM02(공통 UI 컴포넌트 가이드)

### 미착수 화면 (WBS 기준)
- 전 화면 기획서 + 와이어프레임 완료 (Phase 1~4 기획 단계 100%)
- 다음 단계: 개발 착수 (Phase 5~9)
