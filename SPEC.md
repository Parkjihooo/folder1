# 카리 AI 학습 솔루션 (국어) - 개발 명세서

> **문서 버전:** v1.0
> **작성일:** 2026-03-27
> **대상:** 프론트엔드 개발자, 백엔드 개발자
> **현재 상태:** 프로토타입(정적 HTML) → 실서비스 전환 필요

---

## 목차

1. [서비스 개요](#1-서비스-개요)
2. [화면 구성 및 페이지별 명세](#2-화면-구성-및-페이지별-명세)
3. [하드코딩 현황 및 동적 전환 항목](#3-하드코딩-현황-및-동적-전환-항목)
4. [데이터 모델 (DB 스키마)](#4-데이터-모델-db-스키마)
5. [API 명세](#5-api-명세)
6. [AI 연동 명세](#6-ai-연동-명세)
7. [실시간 기능 명세](#7-실시간-기능-명세)
8. [보상/게이미피케이션 시스템](#8-보상게이미피케이션-시스템)
9. [우선순위 및 개발 단계](#9-우선순위-및-개발-단계)

---

## 1. 서비스 개요

### 1-1. 핵심 솔루션 (4가지)

| # | 솔루션 | 설명 | 대상 영역 |
|---|--------|------|----------|
| 1 | **지문 요약 배틀** | 비문학 지문을 읽고 요약 → AI 요약과 비교 평가 | 비문학/독서 |
| 2 | **개념어 스무고개 퀴즈** | 문학 작품 구절 보고 개념어 맞추기 + 이유 서술 | 문학 |
| 3 | **카리 국어 대전** | AI vs 인간 1:1 / 팀전 경쟁 (시즌제) | 전 영역 |
| 4 | **카리 국어 마스터** | 뱃지/카드 수집, 컬렉션 완성 보상 | 전 영역 |

### 1-2. 도입 예정 과목 순서

국어 → 탐구 → 영어 (콘텐츠에 따라 변경 가능)

---

## 2. 화면 구성 및 페이지별 명세

### 2-1. 공통 헤더

| 요소 | 현재 상태 | 서버 연동 필요 |
|------|----------|--------------|
| 로고 | 정적 텍스트 "카리 국어" | - |
| 네비게이션 | 홈, 요약 배틀, 개념어 퀴즈, 국어 대전, 국어 마스터, 학습법 | - |
| 사용자 레벨 | **하드코딩** `Lv.12` | YES - 유저 프로필 API |
| 사용자 닉네임 | **하드코딩** `이.사.독` | YES - 유저 프로필 API |

### 2-2. 홈 페이지

| 요소 | 서버 연동 | 비고 |
|------|----------|------|
| 히어로 문구 | X | 정적 마케팅 텍스트 |
| 4가지 솔루션 카드 | X | 정적 (내부 페이지 링크) |
| 기본 원칙 4가지 | X | 정적 콘텐츠 |

### 2-3. 지문 요약 배틀 페이지

**STEP 1 - 지문 선택**

| 요소 | 현재 상태 | 서버 연동 |
|------|----------|----------|
| 난이도 선택 | 기본/보통/심화 (3개 고정) | 난이도 목록 API or 정적 |
| 지문 유형 선택 | **하드코딩** 철학/과학/경제/사회 (4개) | YES - 지문 유형 목록 API |
| "지문 불러오기" 버튼 | 고정된 1개 지문만 표시됨 | YES - 지문 랜덤 조회 API |

**STEP 2 - 지문 읽기 & 요약**

| 요소 | 현재 상태 | 서버 연동 |
|------|----------|----------|
| 지문 제목 | **하드코딩** "에피쿠로스의 쾌락주의" | YES - 지문 API 응답에 포함 |
| 지문 본문 (문단별) | **하드코딩** 3문단 고정 | YES - 지문 API 응답에 포함 |
| 사용자 요약 입력 | textarea (자유 입력) | YES - 요약 제출 API |

**STEP 3 - AI 비교 결과**

| 요소 | 현재 상태 | 서버 연동 |
|------|----------|----------|
| 나의 요약 표시 | 사용자 입력 그대로 표시 | 프론트 처리 |
| AI 버전 구조적 요약 | **하드코딩** (에피쿠로스 전용) | YES - AI 요약 생성 API |
| 핵심어 포함 점수 | **하드코딩** `85` (+ 랜덤 보정) | YES - AI 채점 API |
| 논리적 관계 점수 | **하드코딩** `72` (+ 랜덤 보정) | YES - AI 채점 API |
| 압축률 점수 | **하드코딩** `80` (+ 랜덤 보정) | YES - AI 채점 API |
| 종합 점수 | 3개 평균 계산 (프론트) | 서버에서 계산 권장 |
| 배틀 승패 결과 | **하드코딩** "사용자 승리" | YES - 채점 API 응답에 포함 |
| 피드백 - 잘하신 점 | **하드코딩** 고정 텍스트 | YES - AI 피드백 생성 API |
| 피드백 - 보완 팁 | **하드코딩** 고정 텍스트 | YES - AI 피드백 생성 API |

### 2-4. 개념어 스무고개 퀴즈 페이지

**STEP 1 - 카테고리 선택**

| 요소 | 현재 상태 | 서버 연동 |
|------|----------|----------|
| 카테고리 목록 | **하드코딩** 표현법/화자의 태도/갈래&특징/수능 빈출 (4개) | YES - 카테고리 목록 API |
| 카테고리별 아이콘 | 이모지 하드코딩 | 서버 or 정적 |
| 카테고리별 설명 | **하드코딩** | YES - 카테고리 API에 포함 |

**STEP 2 - 퀴즈 풀기**

| 요소 | 현재 상태 | 서버 연동 |
|------|----------|----------|
| 인용 구절 | **하드코딩** 신경림 <가난한 사랑 노래> | YES - 퀴즈 문제 API |
| 출처 (작가, 작품명) | **하드코딩** | YES - 퀴즈 문제 API에 포함 |
| 질문 텍스트 | **하드코딩** | YES - 퀴즈 문제 API에 포함 |
| 선택지 목록 (3개) | **하드코딩** 반어법/설의법/역설법 | YES - 퀴즈 문제 API에 포함 |
| 정답 인덱스 | **하드코딩** `1` (설의법) | YES - 서버에서 관리 |
| 이유 서술 입력 | textarea | YES - 답안 제출 API |

**STEP 3 - 정답 확인 & 해설**

| 요소 | 현재 상태 | 서버 연동 |
|------|----------|----------|
| 정답 여부 피드백 | **하드코딩** "정답입니다!" | YES - 채점 API 응답 |
| 이유 서술 평가 | 없음 (미구현) | YES - AI가 이유 서술 분석 |
| 개념어 상세 해설 | **하드코딩** 설의법 vs 반어법 | YES - 해설 API |
| 꿀팁 (혼동 주의) | **하드코딩** | YES - 해설 API에 포함 |
| 함정 주의보 | **하드코딩** | YES - 해설 API에 포함 |

### 2-5. 카리 국어 대전 페이지

| 요소 | 현재 상태 | 서버 연동 |
|------|----------|----------|
| 시즌 번호 | **하드코딩** "시즌 3" | YES - 시즌 정보 API |
| 시즌 이름 | **하드코딩** "독해의 제왕" | YES - 시즌 정보 API |
| 시즌 보상 설명 | **하드코딩** | YES - 시즌 정보 API |
| 시즌 남은 시간 | **하드코딩** 12일 06시간 34분 | YES - 시즌 종료 시각 기반 계산 |
| 1:1 대전 모드 설명 | 정적 텍스트 | - |
| 팀전 모드 설명 | 정적 텍스트 + "준비 중" 표시 | - |
| 랭킹 1~5위 닉네임 | **하드코딩** 5명분 | YES - 랭킹 조회 API |
| 랭킹 승률 | **하드코딩** 94%/91%/88%/85%/82% | YES - 랭킹 API |
| 랭킹 연승 | **하드코딩** 12/8/6/5/4회 | YES - 랭킹 API |
| 랭킹 포인트 | **하드코딩** 2840~2190 pt | YES - 랭킹 API |

### 2-6. 카리 국어 마스터 페이지

**뱃지 컬렉션**

| 요소 | 현재 상태 | 서버 연동 |
|------|----------|----------|
| 뱃지 목록 (6개) | **하드코딩** | YES - 뱃지 목록 API |
| 뱃지 획득 여부 | **하드코딩** earned/locked 2개씩 | YES - 유저 뱃지 API |
| 뱃지 진행률 (%) | **하드코딩** 100%/100%/65%/38%/12%/20% | YES - 유저 뱃지 API |
| 뱃지 조건 텍스트 | **하드코딩** | YES - 뱃지 목록 API |
| 뱃지 이름/설명/대사 | **하드코딩** | YES - 뱃지 목록 API |

**컬렉션 세트**

| 요소 | 현재 상태 | 서버 연동 |
|------|----------|----------|
| 세트 목록 (3개) | **하드코딩** 표현법/태도/비문학 | YES - 컬렉션 세트 API |
| 세트 완성 현황 | **하드코딩** "1 / 3 완성" | YES - 유저 컬렉션 API |
| 세트 내 뱃지 획득 여부 | **하드코딩** | YES - 유저 컬렉션 API |
| 세트 보상 설명 | **하드코딩** | YES - 컬렉션 세트 API |

### 2-7. 학습법 가이드 페이지

| 요소 | 서버 연동 | 비고 |
|------|----------|------|
| 탭 6개 (개요/비문학/문학/고전/선택과목/문해력) | X | 정적 콘텐츠 (CMS 연동 가능) |
| 국어 vs 수학 비교표 | X | 정적 |
| 영역별 핵심 전략표 | X | 정적 |
| 각 탭별 학습법 리스트 | X | 정적 (추후 CMS 관리 고려) |

---

## 3. 하드코딩 현황 및 동적 전환 항목

### 3-1. 반드시 서버 연동 (핵심 기능)

| # | 항목 | 현재 값 | 필요 API | 우선순위 |
|---|------|--------|---------|---------|
| 1 | 지문 데이터 (제목, 본문, 문단) | 에피쿠로스 1개 고정 | `GET /passages/random` | **P0** |
| 2 | AI 요약 결과 | 에피쿠로스 전용 고정 텍스트 | `POST /battles/evaluate` | **P0** |
| 3 | 채점 점수 (핵심어/논리/압축률) | 랜덤 숫자 생성 | `POST /battles/evaluate` | **P0** |
| 4 | 승패 판정 | 항상 "승리" 고정 | `POST /battles/evaluate` | **P0** |
| 5 | 피드백 (잘한점/보완점) | 고정 텍스트 | `POST /battles/evaluate` | **P0** |
| 6 | 퀴즈 문제 (구절/선택지/정답) | 1문제 고정 | `GET /quizzes/random` | **P0** |
| 7 | 퀴즈 해설 | 설의법 고정 | `POST /quizzes/evaluate` | **P0** |
| 8 | 이유 서술 AI 분석 | 미구현 | `POST /quizzes/evaluate` | **P0** |
| 9 | 유저 프로필 (레벨, 닉네임) | Lv.12, 이.사.독 | `GET /users/me` | **P1** |
| 10 | 랭킹 데이터 (5명) | 더미 데이터 | `GET /rankings` | **P1** |
| 11 | 시즌 정보 (번호/이름/종료일) | 시즌3 고정 | `GET /seasons/current` | **P1** |
| 12 | 뱃지 목록 + 유저 진행률 | 6개 고정 | `GET /badges`, `GET /users/me/badges` | **P1** |
| 13 | 컬렉션 세트 + 완성 현황 | 3개 고정 | `GET /collections` | **P1** |

### 3-2. 정적 유지 가능 (추후 CMS 전환 고려)

| 항목 | 비고 |
|------|------|
| 홈 히어로 텍스트 | 마케팅 문구 |
| 기본 원칙 4가지 | 기획 콘텐츠 |
| 학습법 가이드 전체 | 교육 콘텐츠 (추후 CMS) |
| 난이도 옵션 (기본/보통/심화) | enum 수준 |
| 지문 유형 (철학/과학/경제/사회) | 추후 유형 추가 시 API 전환 |
| 퀴즈 카테고리 (4개) | 추후 추가 시 API 전환 |

---

## 4. 데이터 모델 (DB 스키마)

### 4-1. User (사용자)

```
User {
  id            : UUID (PK)
  email         : String (unique)
  nickname      : String          // ex. "이.사.독"
  level         : Integer         // ex. 12
  exp           : Integer         // 경험치
  total_points  : Integer         // 누적 포인트
  season_points : Integer         // 시즌 포인트
  created_at    : DateTime
  updated_at    : DateTime
}
```

### 4-2. Passage (지문)

```
Passage {
  id            : UUID (PK)
  title         : String          // ex. "에피쿠로스의 쾌락주의"
  category      : Enum            // philosophy | science | economy | society
  difficulty    : Enum            // easy | medium | hard
  paragraphs    : JSON Array      // [{ order: 1, content: "..." }, ...]
  paragraph_count : Integer       // 문단 수
  keywords      : String[]        // 핵심 키워드 목록 (채점용)
  ai_summary    : JSON            // { paragraphs: [...], overall: "..." }
  source        : String?         // 출처 (수능 기출 등)
  is_active     : Boolean
  created_at    : DateTime
}
```

**paragraphs JSON 예시:**
```json
[
  { "order": 1, "content": "흔히 '쾌락주의'라고 하면..." },
  { "order": 2, "content": "에피쿠로스에게 쾌락이란..." },
  { "order": 3, "content": "결국 에피쿠로스의 철학은..." }
]
```

**ai_summary JSON 예시:**
```json
{
  "paragraphs": [
    { "order": 1, "label": "개념의 재정의", "content": "일반적인 쾌락과 달리..." },
    { "order": 2, "label": "방법론(아타락시아)", "content": "이성적 사유를 통해..." },
    { "order": 3, "label": "지향점", "content": "감각적 향락이 아닌..." }
  ],
  "overall": "에피쿠로스는 공포와 욕망을 제거하여..."
}
```

### 4-3. Battle (요약 배틀 기록)

```
Battle {
  id              : UUID (PK)
  user_id         : UUID (FK → User)
  passage_id      : UUID (FK → Passage)
  user_summary    : Text           // 사용자가 작성한 요약
  score_keyword   : Integer (0~100)  // 핵심어 포함 점수
  score_logic     : Integer (0~100)  // 논리적 관계 점수
  score_compress  : Integer (0~100)  // 압축률 점수
  score_total     : Integer (0~100)  // 종합 점수
  result          : Enum           // win | lose | draw
  feedback_good   : Text           // AI 피드백 - 잘한 점
  feedback_tip    : Text           // AI 피드백 - 보완 팁
  season_id       : UUID? (FK → Season)
  created_at      : DateTime
}
```

### 4-4. QuizCategory (퀴즈 카테고리)

```
QuizCategory {
  id          : UUID (PK)
  name        : String       // ex. "표현법"
  icon        : String       // ex. "📝"
  description : String       // ex. "설의법, 반어법, 역설법 등"
  sort_order  : Integer
  is_active   : Boolean
}
```

### 4-5. Quiz (퀴즈 문제)

```
Quiz {
  id              : UUID (PK)
  category_id     : UUID (FK → QuizCategory)
  passage_text    : Text            // 인용 구절
  passage_source  : String          // ex. "신경림, <가난한 사랑 노래>"
  question_text   : String          // ex. "밑줄 친 '모르겠는가'에 쓰인 표현 기법은?"
  options         : JSON Array      // [{ index: 0, label: "반어법", desc: "실제 마음과..." }, ...]
  correct_index   : Integer         // 정답 인덱스
  explanation     : Text            // 정답 해설
  tip_title       : String          // 꿀팁 제목
  tip_content     : Text            // 꿀팁 내용
  trap_title      : String?         // 함정 주의보 제목
  trap_content    : Text?           // 함정 주의보 내용
  difficulty      : Enum            // easy | medium | hard
  is_active       : Boolean
  created_at      : DateTime
}
```

**options JSON 예시:**
```json
[
  { "index": 0, "label": "반어법", "description": "실제 마음과 반대로 표현함" },
  { "index": 1, "label": "설의법", "description": "답이 정해져 있는 질문을 통해 의미를 강조함" },
  { "index": 2, "label": "역설법", "description": "모순되는 두 단어를 결합해 진리를 표현함" }
]
```

### 4-6. QuizAttempt (퀴즈 풀이 기록)

```
QuizAttempt {
  id              : UUID (PK)
  user_id         : UUID (FK → User)
  quiz_id         : UUID (FK → Quiz)
  selected_index  : Integer         // 선택한 답
  is_correct      : Boolean
  reason_text     : Text?           // 이유 서술
  reason_feedback : Text?           // AI의 이유 서술 분석 피드백
  created_at      : DateTime
}
```

### 4-7. Season (시즌)

```
Season {
  id           : UUID (PK)
  number       : Integer       // ex. 3
  name         : String        // ex. "독해의 제왕"
  description  : String        // 보상 설명
  start_at     : DateTime
  end_at       : DateTime      // 시즌 종료 시각 (타이머 계산 기준)
  is_active    : Boolean
  rewards      : JSON          // [{ rank_range: "1-3", reward: "카리장학금" }, ...]
}
```

### 4-8. Badge (뱃지)

```
Badge {
  id              : UUID (PK)
  name            : String        // ex. "설의대 수석합격"
  description     : String        // ex. "당연한 걸 물어보는 게 죄인가요?"
  icon            : String        // ex. "🎯"
  condition_text  : String        // ex. "설의법 퀴즈 10회 연속 정답"
  condition_type  : Enum          // streak | count | percentile | custom
  condition_value : JSON          // { type: "quiz_streak", category: "설의법", count: 10 }
  collection_id   : UUID? (FK → Collection)
  sort_order      : Integer
}
```

### 4-9. UserBadge (유저 뱃지 획득)

```
UserBadge {
  id          : UUID (PK)
  user_id     : UUID (FK → User)
  badge_id    : UUID (FK → Badge)
  progress    : Integer (0~100)   // 진행률 (%)
  earned      : Boolean           // 획득 여부
  earned_at   : DateTime?
}
```

### 4-10. Collection (컬렉션 세트)

```
Collection {
  id           : UUID (PK)
  name         : String        // ex. "표현법 세트"
  badge_ids    : UUID[]        // 포함된 뱃지 ID 목록
  reward_text  : String        // ex. "카리 국어 마스터 칭호 + 기프티콘"
  reward_type  : Enum          // title | giftcard | scholarship | goods
  sort_order   : Integer
}
```

### 4-11. Team (팀 - 팀전용)

```
Team {
  id            : UUID (PK)
  name          : String
  leader_id     : UUID (FK → User)
  member_ids    : UUID[]
  season_id     : UUID (FK → Season)
  mission_goal  : JSON          // ex. { type: "avg_accuracy", target: 90 }
  created_at    : DateTime
}
```

### 4-12. Ranking (View / Materialized View)

```
Ranking {
  rank        : Integer
  user_id     : UUID
  nickname    : String
  win_rate    : Float          // 승률 (%)
  win_streak  : Integer        // 현재 연승
  max_streak  : Integer        // 최대 연승
  points      : Integer        // 시즌 포인트
  season_id   : UUID
}
```

---

## 5. API 명세

### 5-1. 인증

| Method | Endpoint | 설명 |
|--------|----------|------|
| POST | `/auth/signup` | 회원가입 |
| POST | `/auth/login` | 로그인 (JWT 발급) |
| POST | `/auth/refresh` | 토큰 갱신 |
| GET | `/auth/me` | 내 정보 조회 |

### 5-2. 사용자

| Method | Endpoint | 설명 | 응답 주요 필드 |
|--------|----------|------|--------------|
| GET | `/users/me` | 내 프로필 | `{ id, nickname, level, exp, total_points, season_points }` |
| PATCH | `/users/me` | 프로필 수정 | `{ nickname }` |
| GET | `/users/me/badges` | 내 뱃지 목록 | `[{ badge_id, name, icon, progress, earned }]` |
| GET | `/users/me/stats` | 내 학습 통계 | `{ battles_count, quiz_count, win_rate, weak_areas[] }` |

### 5-3. 지문 요약 배틀

| Method | Endpoint | 설명 | Request | Response |
|--------|----------|------|---------|----------|
| GET | `/passages/random` | 랜덤 지문 조회 | `?difficulty=medium&category=philosophy` | 아래 참조 |
| POST | `/battles` | 배틀 제출 (요약 + AI 평가 요청) | `{ passage_id, user_summary }` | 아래 참조 |
| GET | `/battles/history` | 내 배틀 히스토리 | `?page=1&limit=20` | `[Battle]` |

**`GET /passages/random` 응답:**
```json
{
  "id": "uuid",
  "title": "에피쿠로스의 쾌락주의",
  "category": "philosophy",
  "difficulty": "medium",
  "paragraphs": [
    { "order": 1, "content": "흔히 '쾌락주의'라고 하면..." }
  ],
  "paragraph_count": 3
}
```

**`POST /battles` 요청:**
```json
{
  "passage_id": "uuid",
  "user_summary": "1문단: 철학자 에피쿠로스가 말한 진정한 쾌락은..."
}
```

**`POST /battles` 응답:**
```json
{
  "id": "uuid",
  "ai_summary": {
    "paragraphs": [
      { "order": 1, "label": "개념의 재정의", "content": "..." }
    ],
    "overall": "에피쿠로스는 공포와 욕망을 제거하여..."
  },
  "scores": {
    "keyword": 85,
    "logic": 72,
    "compression": 80,
    "total": 79
  },
  "result": "win",
  "feedback": {
    "good": [
      { "title": "키워드 적중", "content": "'고통이 없는 상태'... 핵심 단어를 모두 포함" },
      { "title": "인과관계 파악", "content": "'원인을 제거'하면 '결과에 이른다'는 논리 구조..." }
    ],
    "tips": [
      { "title": "대립 구도 파악", "content": "1문단에 나온 '일반적 쾌락' vs '에피쿠로스의 쾌락'..." },
      { "title": "고유 명사 포함", "content": "'아타락시아'라는 고유 명사를 괄호 안에라도..." }
    ]
  },
  "exp_gained": 25,
  "points_gained": 30
}
```

### 5-4. 개념어 퀴즈

| Method | Endpoint | 설명 | Request | Response |
|--------|----------|------|---------|----------|
| GET | `/quiz-categories` | 카테고리 목록 | - | `[{ id, name, icon, description }]` |
| GET | `/quizzes/random` | 랜덤 퀴즈 1문제 | `?category_id=uuid` | 아래 참조 |
| POST | `/quizzes/:id/submit` | 퀴즈 답안 제출 | `{ selected_index, reason_text }` | 아래 참조 |
| GET | `/quizzes/history` | 풀이 히스토리 | `?page=1&limit=20` | `[QuizAttempt]` |

**`GET /quizzes/random` 응답:**
```json
{
  "id": "uuid",
  "category": { "id": "uuid", "name": "표현법" },
  "passage_text": "가난하다고 해서 외로움을 모르겠는가 / 너와 헤어져 돌아오는...",
  "passage_source": "신경림, <가난한 사랑 노래>",
  "question_text": "밑줄 친 '모르겠는가'에 쓰인 표현 기법은 무엇일까요?",
  "options": [
    { "index": 0, "label": "반어법", "description": "실제 마음과 반대로 표현함" },
    { "index": 1, "label": "설의법", "description": "답이 정해져 있는 질문을 통해 의미를 강조함" },
    { "index": 2, "label": "역설법", "description": "모순되는 두 단어를 결합해 진리를 표현함" }
  ]
}
```

> **주의:** `correct_index`는 응답에 포함하지 않음 (정답 노출 방지). 제출 후 응답에서 반환.

**`POST /quizzes/:id/submit` 요청:**
```json
{
  "selected_index": 1,
  "reason_text": "가난하다와 외로움은 모순되는 느낌이 아니라..."
}
```

**`POST /quizzes/:id/submit` 응답:**
```json
{
  "is_correct": true,
  "correct_index": 1,
  "explanation": "단순히 찍은 게 아니라 '상황'과 '논리'를 연결해서...",
  "reason_feedback": "분석 과정이 정말 훌륭해요. 상황과 논리를 연결해서 근거를 찾으셨네요.",
  "tips": [
    {
      "title": "베테랑 AI의 꿀팁 (설의법 vs 반어법)",
      "content": "설의법: 질문의 형식을 빌렸지만..."
    }
  ],
  "traps": [
    {
      "title": "한 단계 더! (함정 주의보)",
      "content": "시험에서는 이렇게 물어볼 수 있어요..."
    }
  ],
  "exp_gained": 15,
  "streak_count": 3
}
```

### 5-5. 대전 / 시즌

| Method | Endpoint | 설명 | Response |
|--------|----------|------|----------|
| GET | `/seasons/current` | 현재 시즌 정보 | `{ id, number, name, description, end_at, rewards[] }` |
| GET | `/rankings` | 시즌 랭킹 | `?season_id=uuid&page=1&limit=20` |
| GET | `/rankings/me` | 내 랭킹 | `{ rank, win_rate, win_streak, points }` |

**`GET /rankings` 응답:**
```json
{
  "rankings": [
    {
      "rank": 1,
      "user": { "id": "uuid", "nickname": "김독해왕", "level": 25 },
      "win_rate": 94.0,
      "win_streak": 12,
      "points": 2840
    }
  ],
  "total_count": 1523,
  "my_rank": 48
}
```

### 5-6. 뱃지 / 컬렉션

| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | `/badges` | 전체 뱃지 목록 (조건 포함) |
| GET | `/users/me/badges` | 내 뱃지 진행률 |
| GET | `/collections` | 컬렉션 세트 목록 |
| GET | `/users/me/collections` | 내 컬렉션 완성 현황 |

### 5-7. 팀 (팀전)

| Method | Endpoint | 설명 |
|--------|----------|------|
| POST | `/teams` | 팀 생성 |
| POST | `/teams/:id/join` | 팀 참가 |
| GET | `/teams/:id` | 팀 정보 조회 |
| GET | `/teams/:id/mission` | 팀 미션 현황 |

---

## 6. AI 연동 명세

### 6-1. 서버에서 AI 호출이 필요한 시점

| 시점 | 입력 | AI가 해야 할 일 | 출력 |
|------|------|----------------|------|
| **배틀 제출** | 지문 원문 + 사용자 요약 | 1) AI 자체 요약 생성<br>2) 사용자 요약 채점 (3개 항목)<br>3) 승패 판정<br>4) 피드백 생성 | 위 `POST /battles` 응답 전체 |
| **퀴즈 이유 서술 평가** | 퀴즈 문제 + 선택 답 + 이유 서술 | 이유 서술의 논리성/정확성 분석 | `reason_feedback` 텍스트 |
| **개념 저장 시 역질문** | 저장하려는 개념 | 해당 개념을 사용자가 직접 설명하도록 역질문 생성 | 역질문 텍스트 (추후 기능) |

### 6-2. 채점 기준 (배틀)

서버/AI에게 전달해야 할 채점 가이드라인:

| 항목 | 기준 | 점수 범위 |
|------|------|----------|
| **핵심어 포함** | 지문에서 생략해서는 안 되는 필수 단어가 포함되었는지 | 0~100 |
| **논리적 관계** | 문장 간의 인과/대립/비례 관계를 정확한 연결어로 서술했는지 | 0~100 |
| **압축률** | 지문의 핵심을 군더더기 없이 짧고 간단하게 요약했는지 | 0~100 |

### 6-3. AI 모델 요구사항

- 지문 요약: 문단별 구조적 요약 + 전체 한 문장 요약 생성 능력
- 채점: 사용자 요약 vs 원문 비교 분석, 정량적 점수 산출
- 피드백: 잘한 점(구체적 근거) + 보완 팁(수능 관점) 생성
- 퀴즈 이유 분석: 학생의 서술 이유로 실제 이해도 판단

---

## 7. 실시간 기능 명세

### 7-1. 필요 시점

| 기능 | 방식 | 우선순위 |
|------|------|---------|
| 시즌 타이머 카운트다운 | 프론트 `setInterval` (시즌 `end_at` 기준) | P1 |
| 랭킹 실시간 갱신 | WebSocket or Polling (30초) | P2 |
| 팀전 미션 현황 | WebSocket | P3 |
| 1:1 대전 매칭 | WebSocket | P3 (추후) |

### 7-2. 시즌 타이머

- 서버에서 `end_at` (ISO 8601)만 내려주면 프론트에서 계산
- 현재는 하드코딩 `12일 06시간 34분`이며, `setInterval(60000)` 으로 분 단위 갱신
- 초 단위 갱신으로 변경 권장 (`setInterval(1000)`)

---

## 8. 보상/게이미피케이션 시스템

### 8-1. 경험치(EXP) 지급 기준

| 활동 | 예상 EXP | 비고 |
|------|---------|------|
| 배틀 완료 | 20~50 | 점수에 비례 |
| 배틀 승리 보너스 | +10 | |
| 퀴즈 정답 | 10~20 | |
| 퀴즈 연속 정답 보너스 | +5 per streak | |
| 이유 서술 우수 | +10 | AI 분석 결과 기반 |

### 8-2. 포인트 지급 (시즌)

| 활동 | 포인트 | 비고 |
|------|--------|------|
| 배틀 승리 | 30 | |
| 배틀 연승 보너스 | +10 per streak | |
| 퀴즈 정답 | 10 | |

### 8-3. 뱃지 달성 조건 (현재 기획 기준)

| 뱃지 이름 | 조건 타입 | 조건 값 |
|----------|---------|--------|
| 설의대 수석합격 | quiz_streak | 설의법 퀴즈 10회 연속 정답 |
| 차가운 장작의 열정 | quiz_count | 역설법 퀴즈 50개 돌파 |
| 참~ 잘했어요 | quiz_percentile | 반어법 퀴즈 상위 1% |
| 카리 속 워런 버핏 | battle_count | 경제 지문 요약 50개 달성 |
| 요약의 신 | battle_streak | 요약 배틀 30연승 |
| 문학 수호자 | quiz_complete | 갈래별 퀴즈 전체 완료 |

### 8-4. 컬렉션 세트

| 세트 이름 | 포함 뱃지 | 완성 보상 |
|----------|---------|---------|
| 표현법 세트 | 설의법 + 역설법 + 반어법 | 카리 국어 마스터 칭호 + 기프티콘 |
| 태도 세트 | 낙관 + 관조 + 의지 | 카리 국어 마스터 칭호 + 굿즈 |
| 비문학 세트 | 경제 + 과학 + 사회 | 카리장학금 교환권 |

---

## 9. 우선순위 및 개발 단계

### Phase 1 - MVP (핵심 학습 기능)

| 항목 | FE | BE |
|------|----|----|
| 회원가입/로그인 | 로그인 UI | Auth API + JWT |
| 지문 DB 구축 (최소 30개) | - | Passage 테이블 + Seed 데이터 |
| 지문 랜덤 조회 | 난이도/유형 필터링 → 동적 렌더링 | `GET /passages/random` |
| 배틀 제출 → AI 채점 | 로딩 상태, 결과 동적 렌더링 | `POST /battles` + AI 연동 |
| 퀴즈 DB 구축 (최소 50개) | - | Quiz 테이블 + Seed 데이터 |
| 퀴즈 랜덤 출제 | 카테고리 필터 → 동적 렌더링 | `GET /quizzes/random` |
| 퀴즈 제출 → 채점 + 해설 | 정답/오답 분기, 해설 렌더링 | `POST /quizzes/:id/submit` |

### Phase 2 - 게이미피케이션

| 항목 | FE | BE |
|------|----|----|
| 유저 프로필 (레벨, 닉네임) | 헤더 동적 바인딩 | User API |
| 뱃지 시스템 | 뱃지 목록/진행률 동적 렌더링 | Badge 달성 조건 체크 로직 |
| 시즌 시스템 | 시즌 정보 + 타이머 | Season API |
| 랭킹 | 랭킹 리스트 동적 렌더링 | Ranking 집계 쿼리 |

### Phase 3 - 소셜/경쟁

| 항목 | FE | BE |
|------|----|----|
| 1:1 대전 매칭 | 매칭 대기 UI | WebSocket 매칭 서버 |
| 팀전 | 팀 생성/참가 UI | Team API + 미션 진행률 |
| 컬렉션 완성 보상 | 보상 수령 UI | 보상 지급 로직 |

### Phase 4 - 고도화

| 항목 | 설명 |
|------|------|
| 리드AI 시선 추적 연동 | 정독/속독 데이터 수집, 히트맵 시각화 |
| 약점 진단 리포트 | 영역별 + 사고 패턴 오류 분석 |
| 학습법 가이드 CMS | 관리자가 학습 콘텐츠 편집 |
| 보상 시스템 실물화 | 기프티콘/장학금 실제 지급 연동 |

---

## 부록: 프론트엔드 전환 체크리스트

현재 정적 HTML에서 프레임워크 전환 시 참고:

- [ ] SPA 프레임워크 선택 (React/Vue/Next.js 등)
- [ ] 페이지 라우팅 전환 (현재 `showPage()` JS → SPA Router)
- [ ] API 클라이언트 세팅 (axios/fetch wrapper + JWT 인터셉터)
- [ ] 상태 관리 세팅 (유저 정보, 시즌 정보 등 전역 상태)
- [ ] 하드코딩 텍스트 → API 응답 바인딩 (위 표의 모든 "YES" 항목)
- [ ] 로딩/에러 상태 UI 추가 (AI 채점은 2~5초 소요 예상)
- [ ] 모바일 반응형 검증 (현재 CSS 기반 → 컴포넌트 레벨 반응형)
- [ ] 타이머 `setInterval` → 초 단위로 변경
- [ ] 점수 랜덤 생성 로직 제거 → 서버 응답값 사용
