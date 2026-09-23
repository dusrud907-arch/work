# [PRD] LUGGAGE PASS (러기지패스) 웹 애플리케이션 제품 요구사항 명세서
**Product Requirement Document: Client-Only Hands-free Travel & Retail Service**

---

## 📌 1. 프로젝트 개요 (Executive Summary)

### 1.1. 서비스 정의
**LUGGAGE PASS(러기지패스)**는 신세계백화점 및 신세계 도심 유통망(이마트24 등)을 기반으로, 한국을 방문한 국내외 관광객이 무거운 짐(캐리어, 쇼핑백)의 구속 없이 자유롭게 쇼핑과 관광을 즐길 수 있도록 지원하는 **외국인 맞춤형 핸즈프리(Hands-free) 스마트 여행 & 모빌리티 솔루션**입니다.

* **브랜드 슬로건:**
  * *"Shop more. Carry less." (사는 건 고객이, 들고 다니는 건 백화점이)*
  * *"두 손은 가볍게, 여행은 더 자유롭게"*
* **서비스 핵심 가치:** 짐 보관(Store) ➔ 매장 쇼핑백 자동 집결(Collect) ➔ 도심 편의점 및 공항 직송(Deliver) ➔ 원스톱 수령(Pick-up)의 완벽한 옴니채널 연계

### 1.2. 프로젝트 개발 방향 및 제약사항
1. **No-Database Architecture (서버 및 DB 미사용):**
   * 별도의 백엔드 데이터베이스 서버 없이 **순수 클라이언트 사이드(Client-Side)**에서 완전 동작.
   * 브라우저 **`localStorage`**, **`sessionStorage`** 및 브라우저 인메모리(In-Memory) 상태 관리를 통해 예약, 보관 상태, 사용자 설정(언어, 다크모드 등)을 영구/세션 저장.
2. **Vanilla JavaScript 중심 구현:**
   * React, Vue 등 무거운 프레임워크 없이 순수 **Vanilla JS (ES6+)**로 컴포넌트화 및 상태 제어 구현.
   * CDN 기반의 경량 라이브러리(Tailwind CSS CDN, Chart.js, PapaParse, Leaflet.js, FontAwesome)를 활용하여 시각적 완성도 극대화.
3. **워크스페이스 데이터(`work` 폴더 자산) 적극 연계:**
   * `work/data/부산상가_편의점.csv`: 신세계 계열 **이마트24 290개 지점**의 위·경도 및 주소 데이터를 파싱하여 실시간 거점 검색 및 지도(Station Finder) 기능 구현.
   * `work/data/01_POS_일별_상품별_판매.csv` & `work/index.html`: 대시보드 UI/UX 패턴을 차용하여 매장/거점별 보관 이용 통계 및 패스 판매 분석 관리자 모드 제공.

---

## 🎯 2. 배경 및 기획 의도 (Background & Opportunity)

### 2.1. 시장 문제점 (Pain Points)
1. **체크아웃 후 이동 제약 (Post-Checkout Burden):**
   * 방한 개별 관광객(FIT)은 숙소 체크아웃(11:00) 후 비행기 탑승(18:00~21:00) 전까지 7~10시간 동안 20~30kg의 대형 캐리어를 소지해야 함.
2. **쇼핑 피로도 급증 및 체류시간 감소:**
   * 백화점 및 도심 상권에서 쇼핑백이 늘어날수록 이동이 불편해져 카페나 휴게 공간에 머무르거나 조기 공항 이동으로 매출 기회 손실.
3. **언어 장벽 및 지하철 물품보관함 부족:**
   * 역사 내 코인락커는 대형 캐리어 수용률이 낮고 잦은 만실 발생. 외국어 안내 부족 및 결제 수단 제약으로 이용 불편.

### 2.2. 타겟 페르소나 (Target Persona)
* **대표 페르소나: 칭칭 (28세, 대만 국적 여성, FIT)**
  * **상황:** 3박 4일 부산·서울 여행 마지막 날. 숙소 체크아웃 완료.
  * **목표:** 센텀시티 신세계백화점과 인근 핫플레이스에서 마지막 쇼핑과 미식을 여유롭게 즐긴 뒤, 김해/인천공항에서 짐을 찾아 바로 출국하고 싶음.
  * **니즈:** 캐리어 보관 + 쇼핑백 들고 다니지 않기 + 비행기 시간에 맞춘 공항 수령 + 다국어(번체 중국어/영어) 지원.

---

## 🗺️ 3. 정보 구조 및 사용자 플로우 (Information Architecture & User Flow)

### 3.1. 사이트 구조 (IA)
```mermaid
graph TD
    A[LUGGAGE PASS 메인 홈] --> B[1. 패스 안내 & 시뮬레이터 (Pass & Pricing)]
    A --> C[2. 거점 찾기 (Station Finder - 이마트24/백화점)]
    A --> D[3. 간편 예약 & 보관 신청 (Instant Booking Wizard)]
    A --> E[4. 실시간 배송/보관 트래킹 (Live Tracker)]
    A --> F[5. 브랜드 스토리 & 홍보관 (Card News Carousel)]
    A --> G[6. 거점 관리자 모드 (Admin & POS Analytics)]
```

### 3.2. 핵심 사용자 시나리오 (End-to-End Flow)
1. **탐색 (Explore):** 메인 랜딩에서 LUGGAGE PASS의 혜택 확인 및 본인 일정에 맞는 패스권(DAY / STAY / PREMIUM) 견적 시뮬레이션.
2. **거점 선택 (Find Station):** 지도 및 검색창에서 현재 위치 인근 '이마트24' 또는 '신세계백화점 웰컴센터' 거점 확인.
3. **예약/체크인 (Check-in Booking):** 수하물 개수, 보관 거점, 수령 장소(공항 데스크 or 백화점) 지정 후 가상 결제 완료 ➔ 고유 예약번호 및 QR 패스 발급.
4. **쇼핑 & 집결 (Hands-Free Shopping):** 백화점에서 쇼핑 시 패스 번호 제시 ➔ 구매 물품이 중앙 집결지로 자동 전달.
5. **실시간 트래킹 (Tracking):** 모바일 웹에서 예약번호 조회로 현재 내 짐의 위치(보관중 ➔ 허브 이동 ➔ 공항 도착 대기) 실시간 확인.
6. **공항 수령 (Pick-up):** 출국장 LUGGAGE PASS 전용 데스크에서 QR 스캔 후 캐리어와 쇼핑백 일괄 수령.

---

## ⚙️ 4. 기술 아키텍처 (No-DB & Client-Side Tech Stack)

### 4.1. 기술 스택
| 구분 | 기술 / 라이브러리 | 선정 사유 및 용도 |
| :--- | :--- | :--- |
| **Markup & Core** | HTML5, Vanilla JavaScript (ES6+) | 프레임워크 종속성 없는 가볍고 빠른 실행, DOM 조작 최적화 |
| **Styling** | Tailwind CSS (CDN) + Vanilla CSS | 프리미엄 럭셔리 톤(신세계 Red & Gold), 다크모드, 반응형 레이아웃 구현 |
| **Data Engine** | PapaParse (5.4.1 CDN) | `부산상가_편의점.csv` (1.3MB) 및 POS 판매 데이터를 브라우저에서 초고속 비동기 파싱 |
| **Data Storage** | Browser `localStorage` / `sessionStorage` | No-DB 환경에서 예약 데이터 영구 보관, 트래킹 상태 갱신, Mock 데이터 저장 |
| **Interactive Map**| Leaflet.js (OpenStreetMap 기반) | API 키 없이 부산 290개 이마트24 거점 마커 표시 및 인터랙티브 지도 렌더링 |
| **Data Viz** | Chart.js | 일별 패스 이용 건수, 거점별 보관 점유율, 매출 통계 시각화 |
| **Icons & Fonts** | FontAwesome 6, Noto Sans KR, Inter | 다국어 타이포그래피 및 직관적 인포그래픽 아이콘 지원 |

### 4.2. LocalStorage 데이터 스키마 정의

#### 1) `luggage_pass_reservations` (예약 목록 테이블 대체)
```json
[
  {
    "bookingId": "LP-20260923-8821",
    "createdAt": "2026-09-23T14:30:00.000Z",
    "customer": {
      "name": "Qing Qing (칭칭)",
      "phone": "+886-912-345-678",
      "email": "qingqing@travel.tw",
      "nationality": "TW"
    },
    "passType": "DAY_PASS",
    "luggageCount": { "carrier": 1, "shoppingBag": 2 },
    "dropoffStation": "이마트24 해운대청사포점",
    "pickupLocation": "김해공항 국제선 LUGGAGE 데스크 (2F)",
    "pickupTime": "2026-09-23 18:30",
    "totalAmount": 20000,
    "paidAmount": 20000,
    "status": "IN_TRANSIT",
    "statusHistory": [
      { "step": "BOOKED", "time": "14:30", "label": "예약 및 결제 완료" },
      { "step": "STORED", "time": "14:45", "label": "거점 위탁 보관 중" },
      { "step": "COLLECTED", "time": "16:00", "label": "백화점 구매품 집결 완료" },
      { "step": "IN_TRANSIT", "time": "16:40", "label": "공항 전용 셔틀 배송 중" },
      { "step": "READY", "time": "", "label": "공항 데스크 수령 대기" },
      { "step": "COMPLETED", "time": "", "label": "수령 완료" }
    ],
    "insuranceApplied": true
  }
]
```

#### 2) `luggage_pass_stations_cache` (이마트24 거점 캐시)
* CSV 파싱 후 필터링된 290개 이마트24 매장의 `{ id, name, roadAddress, lat, lng, district, phone }`을 메모리 및 캐시에 저장하여 검색 속도 0ms 보장.

#### 3) `luggage_pass_app_settings` (사용자 환경설정)
* `{ lang: "ko" | "en" | "zh" | "ja", theme: "light" | "dark", currency: "KRW" }`

---

## 📋 5. 상세 기능 요구사항 (Functional Requirements)

### FR-1. 메인 랜딩 & 브랜드 비주얼 (Hero & Brand Showcase)
* **FR-1.1. 히어로 인터랙션:**
  * 신세계 프리미엄 비주얼과 브랜드 슬로건, 핵심 4대 가치(PAY, COLLECT, STORE, DELIVER) 카드 배치.
  * 우측에 실시간 모의 디지털 패스(Holder: Qing Qing, 잔여금, 실시간 집결 현황, 지닌 짐 무게: 0kg) 위젯 노출.
* **FR-1.2. 3대 홍보물 연계 (카드뉴스 & 포스터 뷰어):**
  * `luggage_pass_promotion_plan.md`의 기획을 반영하여 [문제 환기 ➔ 솔루션 ➔ 핵심 기능 ➔ 이용 프로세스 ➔ 사전 예약 CTA]의 5단계 인터랙티브 카드뉴스 캐러셀 컴포넌트 탑재.
* **FR-1.3. 실시간 다국어 지원 (i18n):**
  * 한국어, English, 繁體中文(대만/홍콩), 日本語 4개 국어 원클릭 토글. 바닐라 JS 딕셔너리로 전체 페이지 텍스트 즉시 전환.

### FR-2. 패스권 라인업 & 스마트 요금 시뮬레이터 (Pass & Pricing Calculator)
* **FR-2.1. 3종 패스권 라인업 비교:**
  * **DAY PASS (20,000원):** 당일 짐 보관(1개) + 백화점 쇼핑백 집결 + 공항 당일 직송 배송.
  * **STAY PASS (35,000원):** 24시간 보관 + 캐리어 2개 + 도심 제휴 호텔 딜리버리.
  * **PREMIUM PASS (50,000원):** 무제한 집결 + 캐리어 2개 + VIP 라운지 이용권 + 5,000만 원 분실/파손 프리미엄 안심 보험.
* **FR-2.2. 충전 금액별 리워드 계산 슬라이더:**
  * 10만 원 ~ 150만 원 선충전 범위 슬라이더 조작 시 등급(Bronze ➔ Silver ➔ Gold ➔ Diamond) 및 즉시 리워드(5% 캐시백, VIP 라운지권, F&B 바우처) 동적 렌더링.

### FR-3. 이마트24 & 백화점 거점 찾기 (Station Finder & Interactive Map)
* **FR-3.1. 부산 지역 이마트24 CSV 데이터 로드:**
  * `work/data/부산상가_편의점.csv` 파일을 PapaParse로 클라이언트에서 스트리밍 파싱.
  * '상호명'에 '이마트24'가 포함된 레코드(290개)를 자동 추출 및 인덱싱.
* **FR-3.2. 인터랙티브 지도 (Leaflet.js):**
  * 부산 전역의 이마트24 거점 및 백화점(센텀시티점) 웰컴센터 마커 핀 표시.
  * 마커 클릭 시 팝업(지점명, 도로명 주소, 보관 가능 여부, '이 거점으로 예약' 버튼) 표출.
* **FR-3.3. 다면 필터 및 검색:**
  * 시군구별 필터 (해운대구, 수영구, 부산진구, 중구 등), 지점명 검색창, 현재 위치 기반 거리순 정렬.
  * 지도 뷰 ⟷ 리스트 카드 뷰 전환 토글 지원.

### FR-4. 원스톱 간편 예약 & 보관 신청 (Instant Booking Wizard)
* **FR-4.1. 4단계 스텝 위저드 (Step-by-step Wizard):**
  * **Step 1:** 패스권 선택 (DAY / STAY / PREMIUM)
  * **Step 2:** 맡길 거점(이마트24 지점 or 센텀시티 웰컴센터) 및 수령 장소(김해공항, 인천공항, 호텔 등), 수령 일시 선택
  * **Step 3:** 고객 정보(성명, 연락처, 이메일, 국적) 및 짐 정보(캐리어 수, 규격, 요청사항) 입력
  * **Step 4:** 모의 결제 (신용카드, 간편결제, 선충전금 차감) 진행 ➔ `localStorage`에 예약 데이터 저장.
* **FR-4.2. 모바일 모바일 바코드/QR 패스 티켓 발급:**
  * 예약 완료 즉시 예약 고유 ID(`LP-YYYYMMDD-XXXX`) 및 Canvas 기반 QR 코드 패스 팝업 생성. 이미지 저장 또는 인쇄 기능 제공.

### FR-5. 실시간 배송/보관 트래킹 (Live Luggage Tracker)
* **FR-5.1. 예약번호 기반 실시간 상태 조회:**
  * 예약번호 입력 시 `localStorage`에서 조회하여 현재 배송 진행 상태를 비주얼 타임라인으로 시각화.
  * 5단계 상태: [1. 접수완료] ➔ [2. 거점 보관중] ➔ [3. 쇼핑백 집결완료] ➔ [4. 배송차량 이동중] ➔ [5. 공항 데스크 도착/수령대기].
* **FR-5.2. 모의 위치 및 안심 보증서:**
  * 배송 차량의 현재 예상 위치(GPS 연출) 및 안전 배송 보증 인증 뱃지 표시.
  * 짐 수령 완료 시 고객 평가(만족도 별점) 및 리뷰 입력 기능.

### FR-6. 거점 관리자 & POS 매출 분석 모드 (Admin & Analytics)
* **FR-6.1. `work/index.html` 기반 대시보드 UI 연계:**
  * 상단 헤더의 '관리자 모드' 진입 버튼을 통해 백화점 웰컴센터 및 이마트24 거점 관리자 뷰 제공.
* **FR-6.2. 실시간 예약 관리 테이블:**
  * `localStorage`에 저장된 전체 예약 목록을 데이터 테이블로 표출 (검색, 상태별 필터, 상태 변경 버튼).
  * 관리자가 '보관중' ➔ '이동중' ➔ '수령완료'로 원클릭 상태 전환 시 고객 트래커에 즉시 반영.
* **FR-6.3. Chart.js 매출 & 통계 차트:**
  * `work/data/01_POS_일별_상품별_판매.csv`의 분석 모델을 벤치마킹하여, 일별 패스권 판매액, 거점별 보관 점유율(파이 차트), 패스 유형별 비중(바 차트) 시각화.

---

## 🎨 6. UI/UX 디자인 시스템 및 인터랙션 가이드

### 6.1. 컬러 팔레트 (신세계 헤리티지 & 럭셔리 모던)
* **Primary Brand:** `#8A1538` (Shinsegae Heritage Red) - 신뢰와 헤리티지
* **Secondary Brand:** `#C5A059` (Champagne Gold) - 프리미엄 케어 및 고급감
* **Accent Color:** `#2563EB` (Tech Cobalt Blue) - 실시간 배송 및 기술 신뢰도
* **Neutral Dark:** `#1A1A1A` (Charcoal Black) - 세련된 텍스트 및 다크모드 배경
* **Neutral Light:** `#F9F9FB` (Soft Slate Gray) - 가독성을 높이는 쾌적한 캔버스
* **Status Success:** `#10B981` (Emerald Green) - 배송 완료, 집결 완료 상태

### 6.2. 디자인 원칙 (Aesthetic Principles)
1. **Glassmorphism & Micro-Interactions:**
   * 반투명 글래스 카드(`backdrop-blur-md`, `rgba(255,255,255,0.85)`), 부드러운 호버 리프트(`translate-y-1`), 탭 전환 애니메이션.
2. **모바일 퍼스트 반응형 레이아웃:**
   * 여행 중 스마트폰으로 접속하는 외국인 관광객 특성을 반영하여 하단 고정 예약 바(Sticky Bottom CTA), 엄지손가락 터치에 최적화된 컨트롤 제공.
3. **직관적인 인포그래픽:**
   * 텍스트를 최소화하고 국가별 언어 장벽이 없도록 국제 표준 수하물 픽토그램(FontAwesome) 적극 활용.

---

## 🔒 7. 비기능적 요구사항 (Non-Functional Requirements)

1. **성능 및 로딩 최적화:**
   * 1.3MB의 CSV(`부산상가_편의점.csv`) 파일은 Web Worker 또는 비동기 Chunk 파싱으로 UI 스레드 멈춤 없이 1초 내에 파싱 및 인메모리 색인 완료.
2. **무설치 및 로컬 파일 실행 보장 (File Protocol Ready):**
   * Node.js 서버나 별도 로컬 웹서버가 없어도 브라우저에서 `index.html`을 더블클릭(`file://`)하여 구동될 수 있도록 CDN 절대 경로 및 유연한 자원 참조 적용.
3. **브라우저 호환성:**
   * Chrome, Edge, Safari, Firefox 최신 버전 및 모바일 브라우저(iOS Safari, Android Chrome) 완벽 대응.
4. **데이터 무결성 및 복구:**
   * LocalStorage가 비어있을 경우 초기 '데모 샘플 데이터(Mock Bookings)'를 자동 시딩(Seeding)하여 즉시 테스트 가능하도록 지원.
   * 언제든지 '샘플 데이터 초기화' 버튼을 통해 초기 상태로 리셋 가능.

---

## 📅 8. 구현 일정 및 마일스톤 (Milestone Roadmap)

| 단계 | 작업 내용 | 산출물 | 완료 기준 |
| :--- | :--- | :--- | :--- |
| **Phase 1** | PRD 작성 및 기능 명세 정의 | `PRD.md` | 사용자 요구사항 반영 및 설계 확정 |
| **Phase 2** | UI 뼈대 및 디자인 시스템 구축 | `index.html`, `style.css` | 모바일/데스크탑 반응형 레이아웃 및 럭셔리 테마 완성 |
| **Phase 3** | CSV 파싱 & 거점 검색 지도 연동 | `app.js`, `storeFinder.js` | 이마트24 290개 지점 Leaflet 맵 마커 표시 및 검색 동작 |
| **Phase 4** | 예약 위저드 & 트래킹 & 다국어 | `booking.js`, `tracker.js`, `i18n.js` | LocalStorage 기반 예약 저장, 티켓 생성, 상태 추적 완료 |
| **Phase 5** | 관리자 대시보드 & 통계 차트 구현 | `admin.js`, `chart.js` | Chart.js 기반 통계 시각화 및 예약 상태 변경 기능 검증 |
| **Phase 6** | 종합 검증, QA 및 문서화 | `walkthrough.md` | 전체 시나리오 브라우저 테스트 및 산출물 보고 |

---

## 💡 9. 결론 및 기대 효과 (Expected Outcome)

본 LUGGAGE PASS 웹 애플리케이션은 **데이터베이스 없이 순수 바닐라 자바스크립트**만으로 구축되지만, 브라우저의 최신 기술(`localStorage`, File API/PapaParse, Leaflet, Canvas)을 유기적으로 조합하여 **실제 상용 서비스에 준하는 고성능·고품질의 디지털 여행 리테일 플랫폼**을 구현합니다.

신세계백화점의 쇼핑 인프라와 부산 전역 290개 이마트24 거점을 하나로 묶어, 외국인 관광객에게는 **'가장 편안한 마지막 날의 여행'**을, 백화점과 유통사에는 **'체류 시간 및 객단가 극대화'**라는 실질적 비즈니스 가치를 창출할 것입니다.
