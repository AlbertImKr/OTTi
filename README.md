# 🎬 OTTi - OTT 통합 콘텐츠 추천 AI Agent

> "오늘 뭐 볼지 고민하는 시간을 줄여주는 AI"

**OTTi**(OTT + AI)는 여러 OTT 플랫폼에 흩어진 콘텐츠를 한 곳에서 대화형으로 추천받을 수 있는 AI Agent 서비스입니다.
넷플릭스, 쿠팡플레이, 디즈니+, 웨이브, 왓챠 등 구독 중인 OTT를 기반으로 취향에 맞는 콘텐츠를 찾아드립니다.

<!-- 스크린샷/GIF 추가 예정 -->
<!-- ![demo](./docs/demo.gif) -->

---

## ✨ 주요 기능

### 🗣️ 대화형 추천
자연어로 원하는 콘텐츠를 요청하면 AI가 조건을 분석해 맞춤 추천합니다.

```
사용자: "넷플릭스에서 볼 만한 스릴러 추천해줘, 평점 높은 거"
Agent:  장르, OTT, 평점 조건을 분석해서 최적의 콘텐츠 5개를 추천드립니다!
```

### 🎯 OTT 필터링
구독 중인 OTT만 필터링해서 바로 볼 수 있는 콘텐츠만 추천합니다.

### 😊 감정 기반 추천
기분이나 상황을 말하면 그에 맞는 콘텐츠를 찾아줍니다.

```
사용자: "오늘 힘든 하루였어. 기분 풀리는 거 뭐 없을까"
Agent:  힐링되는 코미디/드라마를 골라드릴게요!
```

### 🔗 유사 콘텐츠 탐색
좋아하는 작품과 비슷한 콘텐츠를 찾아줍니다.

```
사용자: "기생충 같은 영화 더 없어?"
Agent:  비슷한 분위기의 작품 5개를 추천합니다!
```

### 📝 시청 기록 관리
봤던 작품을 기록하고, 중복 추천을 방지합니다.

---

## 🏗️ 시스템 아키텍처

```mermaid
graph TD
    subgraph "Frontend (React)"
        A["채팅 UI + 콘텐츠 카드 + 필터"]
    end

    subgraph "API Gateway (Spring Boot 4)"
        B["인증 / 사용자 / 콘텐츠 / 시청기록 관리"]
    end

    subgraph "AI Agent (Python / FastAPI)"
        C["LangChain + Tool Calling + 추천 엔진"]
    end

    A <-->|REST API| B
    B <-->|gRPC / REST| C

    C --> D["OTT API (TMDB/JustWatch)"]
    C --> E["Claude API (LLM)"]
    C --> G["Ollama (Local LLM)"]
    B --> F[("PostgreSQL (pgvector) + Redis")]
```

---

## 🛠️ 기술 스택

### Backend
![Java](https://img.shields.io/badge/Java_21-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_4-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring_Security-6DB33F?style=for-the-badge&logo=spring-security&logoColor=white)
![JPA](https://img.shields.io/badge/JPA/Hibernate-59666C?style=for-the-badge&logo=hibernate&logoColor=white)

### AI Agent
![Python](https://img.shields.io/badge/Python_3.11-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-Local_LLM-000000?style=for-the-badge&logo=ollama&logoColor=white)

### Frontend
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwind-css&logoColor=white)

### Infra & DB
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![pgvector](https://img.shields.io/badge/pgvector-Vector_DB-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

### External APIs
![TMDB](https://img.shields.io/badge/TMDB_API-01B4E4?style=for-the-badge&logo=themoviedatabase&logoColor=white)
![Claude](https://img.shields.io/badge/Claude_API-191919?style=for-the-badge&logo=anthropic&logoColor=white)

---

## 🤖 AI Agent 동작 방식

사용자의 자연어 입력을 LLM이 분석하고, 적절한 Tool을 호출하여 결과를 조합합니다.

```mermaid
sequenceDiagram
    participant U as 사용자
    participant A as AI Agent (Claude)
    participant T as Tools (OTT API/DB)

    U->>A: "오늘 볼만한 스릴러 추천해줘"
    Note over A: 의도 분석 (Intent Classification)
    A->>T: search_content(genre='thriller')
    T-->>A: 콘텐츠 리스트 반환
    A->>T: check_ott_availability(contents)
    T-->>A: OTT 플랫폼 정보 반환
    Note over A: 응답 생성 (Response Generation)
    alt Local LLM
        A->>O: Ollama Llama3 추론
        O-->>A: 응답 생성
    else Cloud API
        A->>C: Claude 3.5 Sonnet 추론
        C-->>A: 응답 생성
    end
    A->>U: 추천 결과 및 상세 정보 제공
```

### Agent Tools

| Tool | 역할 |
|------|------|
| `search_content` | 장르, 평점, 연도 등 조건 기반 콘텐츠 검색 |
| `get_similar` | 특정 작품과 유사한 콘텐츠 조회 |
| `get_detail` | 작품 상세 정보 (줄거리, 출연진, 평점) |
| `check_ott_availability` | OTT 플랫폼별 제공 여부 확인 |
| `get_trending` | 현재 인기/트렌딩 콘텐츠 조회 |
| `save_watch_history` | 사용자 시청 기록 저장 |

---


## 📁 프로젝트 구조

```
otti/
├── backend/                    # Spring Boot API 서버
│   ├── src/main/java/
│   │   └── com/otti/
│   │       ├── auth/           # 인증 (JWT, Spring Security)
│   │       ├── user/           # 사용자 관리
│   │       ├── content/        # 콘텐츠 (TMDB 연동)
│   │       ├── subscription/   # OTT 구독 관리
│   │       ├── history/        # 시청 기록
│   │       └── chat/           # 대화 세션 관리
│   └── build.gradle
│
├── ai-agent/                   # Python AI Agent 서버
│   ├── agent/
│   │   ├── agent.py            # LangChain Agent 메인
│   │   ├── tools/              # Agent Tools
│   │   │   ├── search.py       # 콘텐츠 검색
│   │   │   ├── similar.py      # 유사 콘텐츠
│   │   │   ├── detail.py       # 상세 정보
│   │   │   ├── ott.py          # OTT 필터링
│   │   │   └── trending.py     # 트렌딩
│   │   └── prompts/            # 시스템 프롬프트
│   ├── main.py                 # FastAPI 엔트리포인트
│   └── requirements.txt
│
├── frontend/                   # React 프론트엔드
│   ├── src/
│   │   ├── components/
│   │   │   ├── Chat/           # 채팅 UI
│   │   │   ├── ContentCard/    # 콘텐츠 카드
│   │   │   └── common/         # 공통 컴포넌트
│   │   ├── pages/              # 페이지
│   │   └── api/                # API 호출
│   └── package.json
│
├── docker-compose.yml
└── README.md
```

---

## 📊 데이터베이스 설계 (ERD)

```mermaid
erDiagram
    USERS {
        long id PK
        string email
        string password
        string nickname
        datetime created_at
    }
    USER_SUBSCRIPTIONS {
        long id PK
        long user_id FK
        string ott_name
        datetime created_at
    }
    WATCH_HISTORY {
        long id PK
        long user_id FK
        string tmdb_id
        string content_type
        int rating
        datetime watched_at
    }
    CHAT_SESSIONS {
        long id PK
        long user_id FK
        datetime created_at
    }
    CHAT_MESSAGES {
        long id PK
        long session_id FK
        string role
        text content
        datetime created_at
    }

    USERS ||--o{ USER_SUBSCRIPTIONS : "has"
    USERS ||--o{ WATCH_HISTORY : "records"
    USERS ||--o{ CHAT_SESSIONS : "starts"
    CHAT_SESSIONS ||--o{ CHAT_MESSAGES : "contains"
```

---

## 🗓️ 개발 로드맵

### Month 1 - MVP 🔄
- [ ] 프로젝트 세팅 (Spring Boot + FastAPI + React)
- [ ] OTT API 연동
- [ ] 회원가입/로그인 (JWT)
- [ ] AI Agent 개발 (LangChain + Tool Calling)
- [ ] 대화형 추천 기능
- [ ] OTT 필터링
- [ ] 채팅 UI + 콘텐츠 카드
- [ ] Docker Compose 배포

### Month 2 - 고도화 🔄
- [ ] 시청기록 기반 개인화 추천
- [ ] 추천 피드백 (👍👎)
- [ ] 위시리스트 기능
- [ ] 멀티턴 대화 맥락 강화
- [ ] Redis 캐싱 최적화

### Month 3 - 확장 📋
- [ ] 신작 알림 (이메일)
- [ ] 추천 결과 공유 링크
- [ ] 친구 취향 비교 / 그룹 추천
- [ ] OTT 구독 최적화 제안
- [ ] 모바일 반응형 완성

---

## 🎯 시연 시나리오

| 시나리오 | 사용자 입력 | Agent 동작 |
|---------|-----------|-----------|
| 기본 추천 | "넷플릭스에서 스릴러 추천해줘" | TMDB 검색 → OTT 필터 → 5개 추천 |
| 감정 기반 | "오늘 힘든 하루였어" | 감정 파악 → 힐링 콘텐츠 추천 |
| 유사 작품 | "기생충 같은 영화 더 없어?" | 유사 작품 조회 → 추천 |
| 연속 대화 | "그 중에서 쿠팡에서 볼 수 있는 거?" | 이전 대화 맥락 + OTT 필터링 |

---

## 📄 License

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details.

---

<p align="center">
  Made with ❤️ by OTTi
</p>