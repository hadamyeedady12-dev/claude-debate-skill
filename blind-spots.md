# Blind Spot Reference Database

> LLM이 체계적으로 놓치는 관점 25가지. security-reviewer, code-architecture-reviewer 에이전트가
> 리뷰 시 해당 도메인 키워드가 감지되면 이 파일의 프로브 질문을 주입하여 분석 깊이를 높인다.
>
> **출처**: claude-octopus OctoBench v1.0 benchmark data (MIT License)

---

## 1. Security & Threat Modeling

### SEC-01: 서플라이 체인 공격
**키워드**: integration, third-party, vendor, partner, webhook, plugin
**프로브**: 정상적인 통합 파트너가 침해당해 유효한 API 자격증명을 악용하면? 유효 토큰을 통한 측면 이동을 어떻게 탐지하고 격리하는가?

### SEC-02: 자격증명 침해 탐지
**키워드**: credential, key, token, rotation, leak, secret
**프로브**: 유출된 자격증명을 어떻게 발견하는가(GitHub secret scanning, 다크웹 모니터링)? 폐기가 자동인가 수동인가? 퍼블릭 레포에 자격증명이 노출된 후 평균 폐기 시간은?

### SEC-03: 인증 에러 분류 체계
**키워드**: auth, error, debug, 401, 403, troubleshoot
**프로브**: 소비자가 401(잘못된 자격증명)과 403(권한 부족)과 429(속도 제한)를 어떻게 구분하는가? 디버깅용 토큰 인트로스펙션 엔드포인트가 있는가?

### SEC-04: 고객 보안 감사
**키워드**: enterprise, b2b, compliance, audit, soc, questionnaire
**프로브**: B2B/엔터프라이즈 고객이 200+ 질문의 보안 설문지와 펜테스트 보고서, SOC2 Type II 증거를 요구하면? 이 증거를 효율적으로 생산할 수 있는 아키텍처인가?

### SEC-05: 단일 엔드포인트 보안 인프라 비호환
**키워드**: graphql, gateway, waf, firewall, rate-limit, single-endpoint
**프로브**: 단일 엔드포인트(예: GraphQL POST /graphql) 사용 시 전통적 보안 인프라가 어떻게 깨지는가: 경로 기반 WAF 규칙, 엔드포인트별 속도 제한, URL 기반 접근 로깅, CDN 캐싱 모두 고유 URL 경로를 가정한다.

---

## 2. Architecture & System Design

### ARCH-01: 서비스 간 신뢰 경계
**키워드**: microservice, service-to-service, internal, m2m, machine
**프로브**: 마이크로서비스가 서로를 어떻게 인증하는가? 워크로드 아이덴티티(SPIFFE/SPIRE)가 있는가? 내부 트래픽이 암호화(mTLS)되는가? 침해된 내부 서비스가 다른 서비스를 사칭하는 것을 무엇이 방지하는가?

### ARCH-02: 결정 가역성
**키워드**: migration, transition, rewrite, refactor, adopt, move-to
**프로브**: 마이그레이션이 중간에 실패하면 되돌릴 수 있는가? 각 단계에서의 복귀 비용은? 명시적 중단 기준을 정의하라. 이전과 새 시스템이 공존하는 병렬 실행 기간이 있는가?

### ARCH-03: 교체 전 점진적 개선
**키워드**: graphql, over-fetch, under-fetch, n+1, rest-api, replace
**프로브**: 전면 교체를 권고하기 전에, 기존 시스템의 타겟 개선으로 문제의 80%를 비용의 10%로 해결할 수 있는지 평가하라. '개선된 현재'와 '전면 교체' 간 격차를 정량화하라.

### ARCH-04: 스키마 진화 (마이그레이션 이후)
**키워드**: schema, migration, graphql, api-version, breaking-change
**프로브**: 초기 마이그레이션 너머를 보라: 지속적 진화를 어떻게 처리하는가? 필드 폐기, 추가 전용 변경, 계약 테스트. 마이그레이션은 일회성 비용이지만 진화는 영구적이다.

### ARCH-05: 계약적 API 안정성
**키워드**: enterprise, client, b2b, sla, contract, customer
**프로브**: 계약적 API 안정성 보장이 있는 클라이언트가 있는가? 강제 마이그레이션이 기존 계약을 위반할 수 있다. 계약 제약과 이를 깨는 법적/상업적 비용을 식별하라.

---

## 3. Data Engineering & Storage

### DATA-01: 캐시 무효화 폭발 반경
**키워드**: cache, redis, memcache, invalidation, ttl, eviction
**프로브**: 단일 캐시 플러시가 데이터베이스에 대한 썬더링 허드를 유발할 수 있는가? 캐시 미스와 데이터베이스 쿼리 사이에 단계적 무효화나 서킷 브레이커가 있는가?

### DATA-02: 백업 복원 테스트
**키워드**: backup, restore, disaster, recovery, rpo, rto, replicate
**프로브**: 백업이 실제로 테스트되는가? 마지막 전체 복원은 언제 수행되었고 얼마나 걸렸는가? 측정된(이론적이 아닌) RTO는? 복원 프로세스에 한 사람만 아는 수동 단계가 필요한가?

### DATA-03: 장기 꼬리 데이터 마이그레이션
**키워드**: migration, schema-change, data-model, transform, etl
**프로브**: 새 스키마로 깔끔하게 변환되지 않는 레코드의 비율은? 고아 레코드, null 외래 키, 역사적으로 비일관적인 데이터를 어떻게 처리하는가? 실패용 격리 테이블이 있는가?

### DATA-04: 테넌트 간 데이터 유출
**키워드**: multi-tenant, tenant, shared-database, isolation, row-level
**프로브**: 쿼리가 tenant_id WHERE 절을 빠뜨리면 어떻게 되는가? 테넌트 격리가 데이터베이스 수준(RLS)에서 강제되는가 아니면 애플리케이션 수준에서만? 어떤 쿼리도 교차 테넌트 데이터를 반환할 수 없음을 검증하는 자동화 테스트가 있는가?

### DATA-05: GDPR/CCPA 삭제 전파
**키워드**: gdpr, ccpa, deletion, right-to-erasure, pii, personal-data
**프로브**: 삭제 권리를 위해: PII가 존재하는 모든 위치를 매핑하라 — 기본 DB, 읽기 복제본, 검색 인덱스, 캐시, 분석 파이프라인, 이벤트 스트림, 백업, 서드파티 서비스. 삭제가 모든 위치에 전파되는 데 얼마나 걸리는가?

---

## 4. API Design & Integration

### API-01: 인증 메커니즘 변경 = API 버전 경계
**키워드**: version, auth, breaking-change, v2, api-version
**프로브**: 인증 메커니즘 변경을 API 버전 경계에 묶어야 하는가? API 키에서 OAuth2로 이동하는 것이 v1→v2 전환인가, 아니면 전환 기간 동안 인증과 API 버전이 독립적인가?

### API-02: 벤더 가격 절벽
**키워드**: auth0, okta, cognito, clerk, pricing, vendor, saas, build-vs-buy
**프로브**: 서드파티 벤더를 권고한다면, 실제 규모에서의 가격을 모델링하라. 많은 벤더가 급격한 티어 절벽을 가진다(예: Auth0 무료→10K MAU에서 $23K/년, 엔터프라이즈에서 $100K+). 예상 볼륨에 대한 구체적 숫자를 구하라.

### API-03: 좀비 통합
**키워드**: migration, client, integration, deprecation, sunset, enterprise
**프로브**: 좀비 통합을 고려하라: 일부 엔터프라이즈 클라이언트는 더 이상 존재하지 않는 팀이 유지보수하는 통합 코드를 가지고 있다. 이 통합은 절대 자발적으로 마이그레이션하지 않을 것이다. 장기 꼬리 전략은?

### API-04: 아웃바운드 웹훅 인증
**키워드**: webhook, callback, event, notification, outbound
**프로브**: API가 아웃바운드 웹훅을 보낸다면, 웹훅 인증을 인바운드 API 인증과 별도로 다루라. 아웃바운드 웹훅은 HMAC 요청 서명을 사용한다(OAuth/JWT가 아닌) — 근본적으로 다른 인증 패턴. 서명 알고리즘, 리플레이 보호, 시크릿 로테이션을 다루라.

### API-05: 섀도우 트래픽 검증
**키워드**: migration, test, validation, parity, parallel-run
**프로브**: API 마이그레이션에서 섀도우 트래픽 검증을 고려하라: 프로덕션 트래픽을 이전 및 새 API 모두에 리플레이하고 응답을 diff하라. 이것은 유닛 테스트가 놓치는 불일치를 잡는다. '패리티'의 의미를 정의하라 — 정확 일치인가 허용 가능한 발산 임계값인가.

---

## 5. Operations, SRE & Deployment

### OPS-01: 스키마 분기 후 롤백 불가
**키워드**: rollback, deploy, migration, canary, blue-green, revert
**프로브**: 롤백 가능성을 평가하라: 배포에 데이터베이스 마이그레이션(새 컬럼, 변경된 제약조건)이 포함되면, 데이터베이스를 롤백하지 않고 애플리케이션 코드를 롤백할 수 있는가? 순방향 전용 스키마 변경은 코드 롤백을 안전하지 않게 만든다.

### OPS-02: 메트릭 카디널리티 폭발
**키워드**: metric, monitor, observability, prometheus, datadog, grafana, label
**프로브**: 메트릭 카디널리티를 다루라: customer_id × endpoint × status_code × region의 레이블이 75K+ 고유 시계열을 생성할 수 있다. 모니터링 시스템은 특정 카디널리티 임계값을 넘으면 성능이 저하된다. 카디널리티 예산이 있는가?

### OPS-03: 인시던트 대응 인적 요소
**키워드**: incident, on-call, runbook, pager, alert, escalation
**프로브**: 알림 너머의 인적 요소를 다루라: 런북이 현재 상태이고 테스트되었는가? 온콜이 지속 가능한가(주당 페이지 수, 알림 피로)? 버스 팩터는 — 한 명 이상이 이 시스템을 진단하고 수정할 수 있는가?

### OPS-04: CI/CD 파이프라인 공격 표면
**키워드**: ci, cd, pipeline, github-action, jenkins, deploy, artifact
**프로브**: CI/CD를 특권 공격 표면으로 취급하라: 프로덕션에 대한 쓰기 접근 권한을 가지고 서드파티 코드를 실행한다. 파이프라인 시크릿이 좁게 범위 지정되어 있는가? 침해된 GitHub Action이 시크릿을 유출할 수 있는가? 아티팩트 서명(SLSA)이 있는가?

### OPS-05: 클라우드 비용 관측 지연
**키워드**: cost, cloud, aws, gcp, azure, billing, budget, spend
**프로브**: 클라우드 청구는 24-48시간 지연이 있다 — 잘못 구성된 오토스케일러가 누구도 알아차리기 전에 수천 달러를 누적할 수 있다. 실시간 비용 이상 탐지가 있는가? 하드 지출 제한이 있는가? 요금을 누적하는 좀비 리소스가 있는가?
