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

Claude Code 터미널에서:

```bash
claude plugin marketplace add hadamyeedady12-dev/claude-debate-skill
claude plugin install debate@claude-debate-skill
```

## 사용법

Claude Code 대화에서:

```
/debate:debate React vs Svelte 프로덕션 프론트엔드 선택
/debate:debate 모놀리스 vs 마이크로서비스 전환 시점
/debate:debate REST vs GraphQL API 설계
/debate:debate Kubernetes vs serverless 인프라 전략
```

### 외부 CLI (선택)

Codex, Gemini CLI가 설치되어 있으면 실제 3개 모델이 토론합니다. 없으면 Claude 단독 다관점 분석으로 자동 전환됩니다.

```bash
npm install -g @openai/codex    # Codex CLI
npm install -g @google/gemini-cli  # Gemini CLI
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
│   └── marketplace.json     # 마켓플레이스 정의
├── plugin/
│   ├── commands/
│   │   └── debate.md        # /debate 커맨드
│   └── config/
│       └── blind-spots.md   # LLM 블라인드 스팟 프로브 DB
└── README.md
```

## 라이선스

MIT

## 출처

- [AI Debate Hub](https://github.com/wolverin0/ai-debate-hub) by wolverin0 (MIT)
- Claude Octopus debate enhancements by nyldn (MIT)
