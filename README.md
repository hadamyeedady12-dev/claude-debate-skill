# /debate — Multi-Model Structured Debate

Codex, Gemini, Claude가 라운드별 토론을 수행하는 **Claude Code 플러그인**입니다.

## 뭘 하는 스킬인가요?

하나의 주제에 대해 3개 AI 모델이 각자의 관점에서 분석하고, 서로의 약점을 지적하며, 최종 종합 문서를 만들어 줍니다.

| 참가자 | 관점 |
|--------|------|
| 🔴 Codex | 기술 구현 (코드, 아키텍처, 성능) |
| 🟡 Gemini | 생태계/전략 (커뮤니티, 트렌드, 대안) |
| 🔵 Claude | 독립 분석 + 종합 진행 |

## 설치

### 방법 1: 플러그인으로 설치 (권장)

Claude Code에서 아래 명령어를 실행하세요:

```
/plugin marketplace add hadamyeedady12-dev/claude-debate-skill
/plugin install debate@debate-skill
```

설치 후 바로 사용 가능합니다:

```
/debate:debate React vs Svelte
```

### 방법 2: 수동 설치

```bash
# commands 파일 복사
cp commands/debate.md ~/.claude/commands/debate.md

# blind-spots 데이터베이스 (선택)
mkdir -p ~/.claude/config
cp config/blind-spots.md ~/.claude/config/blind-spots.md
```

수동 설치 시:

```
/debate React vs Svelte
```

### 외부 CLI 설치 (선택)

Codex, Gemini CLI가 있으면 실제 멀티모델 토론이 진행됩니다. 없으면 Claude 단독 다관점 분석으로 자동 전환됩니다.

```bash
# Codex CLI
npm install -g @openai/codex

# Gemini CLI
npm install -g @google/gemini-cli
```

## 사용 예시

```
/debate React vs Svelte 프로덕션 프론트엔드 선택
/debate 모놀리스 vs 마이크로서비스 전환 시점
/debate REST vs GraphQL API 설계
/debate Kubernetes vs serverless 인프라 전략
```

## 토론 흐름

1. **설정 확인** — 목표, 평가 방식, 우선순위, 라운드 수
2. **라운드 1** — 각 모델 독립 분석 (300단어 이내)
3. **라운드 2+** — Cross-critique(상호 비판) 또는 Independent(독립 심화)
4. **품질 평가** — 길이, 근거, 구체성, 참여도 (0-100)
5. **최종 종합** — 합의/분쟁 영역, 권고, 액션 아이템

## 파일 구조

```
claude-debate-skill/
├── .claude-plugin/
│   ├── plugin.json          # 플러그인 매니페스트
│   └── marketplace.json     # 마켓플레이스 정의
├── commands/
│   └── debate.md            # /debate 커맨드
├── config/
│   └── blind-spots.md       # LLM 블라인드 스팟 프로브 DB
└── README.md
```

## 라이선스

MIT

## 출처

- [AI Debate Hub](https://github.com/wolverin0/ai-debate-hub) by wolverin0 (MIT)
- Claude Octopus debate enhancements by nyldn (MIT)
