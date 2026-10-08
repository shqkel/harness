<p align="center">
  <img src="https://img.shields.io/badge/Version-2.0.0%20(fork)-brightgreen.svg" alt="Version">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg" alt="License"></a>
  <img src="https://img.shields.io/badge/Claude_Code-Plugin-purple.svg" alt="Claude Code Plugin">
  <img src="https://img.shields.io/badge/실행모드-3종-teal.svg" alt="3 Execution Modes">
  <img src="https://img.shields.io/badge/패턴-6+품질패턴-orange.svg" alt="Patterns">
</p>

<p align="center">
  <sub><a href="https://revfactory.github.io/harness-animation/">2분짜리 인터랙티브 애니메이션 보기 →</a></sub>
</p>

# Harness v2 — Claude Code를 위한 팀 아키텍처 팩토리

[English](README.md) | **한국어**

> **이 저장소는 [revfactory/harness](https://github.com/revfactory/harness)의 포크입니다.** 원본 v2.1.0을 기반으로 스펙 추적·명명 규칙·`CLAUDE.md` 연결 정보·하네스별 중간 산출물 폴더를 더했습니다. 정확한 차이는 [포크 변경사항](#포크-변경사항--원본-대비-무엇을-더했나)을 보세요.

> **Harness는 Claude Code용 팀 아키텍처 팩토리입니다.** **"하네스 구성해줘"** 한 문장으로, 플러그인이 도메인 설명을 에이전트 팀과 그들이 쓸 스킬로 변환합니다.

## v2에서 달라진 것

v2는 현행 Claude Code 멀티에이전트 런타임에 맞춰 바닥부터 재구축했습니다:

- **3중 네이티브 실행 모드.** v1은 이제 존재하지 않는 실험적 `TeamCreate` API 위에 지어져 있었습니다. v2는 실제로 출시된 프리미티브를 대상으로 합니다:
  1. **워크플로우 오케스트레이션** — 결정적 스크립트(`pipeline()` / `parallel()` / 스키마 / 버짓)로 팬아웃·검증 루프·대규모 실행
  2. **퍼시스턴트 에이전트 협업** — 이름 붙인 에이전트 + `SendMessage` + 공유 태스크, 턴을 넘어 컨텍스트 유지
  3. **서브에이전트 위임** — 경량 단발 병렬 호출
- **워크플로우 네이티브 품질 패턴.** 적대적 검증, 심판 패널, loop-until-dry, 다각 스윕, 완전성 비평가 — 생성된 하네스가 "그럴듯하지만 틀린" 산출물을 걸러내도록 체계화.
- **실험 플래그 완전 제거.** `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` 의존성이 사라졌습니다.
- **합리적 모델 정책.** v1은 모든 에이전트를 `model: "opus"`로 고정했습니다. v2는 업무의 복잡도·작업 기간·자율성·응답 속도에 따라 에이전트별로 fable/opus/sonnet 티어를 선택하며, 무근거 일괄 지정을 금지합니다.
- **`/harness:evolve` 실제 출시.** v1이 문서로만 약속했던 진화 메커니즘이 실제 스킬로 제공됩니다: 초기 구성과 현재 상태의 델타를 포착하고, 피드백을 일반화하여 에이전트·스킬·오케스트레이터에 되먹입니다.
- **v1 마이그레이션 내장.** 팩토리가 v1 산출물(`TeamCreate`, `TeamDelete`, 실험 플래그)을 감지하면 기계적 마이그레이션 경로를 제안합니다.

## 핵심 기능

- **에이전트 팀 설계** — 6가지 아키텍처 패턴(파이프라인, 팬아웃/팬인, 전문가 풀, 생성-검증, 감독자, 계층적 위임), 각 패턴에 최적 실행 모드 매핑
- **스킬 생성** — Progressive Disclosure로 컨텍스트를 효율 관리하는 스킬 자동 생성
- **오케스트레이션** — 데이터 전달 프로토콜(구조화 스키마, 파일, 메시지, 태스크), 에러 핸들링, resume 지원
- **검증 체계** — 트리거 검증, 드라이런, With-skill vs Without-skill A/B 테스트 (A/B 자체를 워크플로우로 구성 가능)
- **진화** — `/harness:evolve`가 사용 피드백을 측정 가능한 다음 세대 개선으로 변환

## 워크플로우

```
Phase 0: 현황 감사 (신규/확장/유지보수 분기 — v1 산출물 감지 포함)
Phase 1: 도메인 분석 (작업의 제어 흐름 형태 포함)
Phase 2: 실행 모드 & 팀 아키텍처 설계
Phase 3: 에이전트 정의 생성 (.claude/agents/)
Phase 4: 스킬 생성 (.claude/skills/)
Phase 5: 오케스트레이션 통합 & CLAUDE.md 포인터
Phase 6: 검증 및 테스트
Phase 7: 운영/유지보수 — 진화는 /harness:evolve
```

## 설치

### 마켓플레이스 설치

```shell
/plugin marketplace add shqkel/harness
/plugin install harness@harness-marketplace
```

### 글로벌 스킬로 직접 설치

```shell
cp -r skills/harness ~/.claude/skills/harness
cp -r skills/evolve ~/.claude/skills/harness-evolve
```

환경 변수나 실험 플래그가 필요 없습니다.

## 사용법

```
하네스 구성해줘
하네스 설계해줘
이 프로젝트에 맞는 에이전트 팀 구축해줘
```

생성된 하네스를 사용한 후:

```
하네스 회고해줘 / 이 피드백 하네스에 반영해줘
```

### 실행 모드 선택

| 모드 | 프리미티브 | 언제 |
|------|-----------|------|
| **워크플로우 오케스트레이션** | `Workflow` 스크립트 | 제어 흐름이 결정적: 열거 가능한 팬아웃, 검증 루프, 대규모, 구조화 출력 |
| **퍼시스턴트 에이전트** | `Agent(name:)` + `SendMessage` + 태스크 | 컨텍스트를 유지하는 장기 전문가, 반복 피드백·협상 |
| **서브에이전트 위임** | 단발 `Agent` 호출 | 결과만 필요한 병렬 위임 |

팩토리는 팀 크기가 아니라 **제어 흐름의 형태**로 모드를 선택하며, Phase별로 모드를 섞는 하이브리드도 지원합니다.

## 산출물

```
프로젝트/
├── .claude/
│   ├── agents/          # 에이전트 정의 (누가)
│   │   ├── analyst.md
│   │   ├── builder.md
│   │   └── qa.md
│   └── skills/          # 스킬 (어떻게) + 오케스트레이터 1개 (누가 언제 어떤 순서로)
│       ├── analyze/SKILL.md
│       └── build/SKILL.md
└── CLAUDE.md            # 최소 포인터: 트리거 규칙 + 변경 이력
```

## v1에서 마이그레이션

[docs/migration-v1-to-v2.md](docs/migration-v1-to-v2.md) 참조. 요약: `TeamCreate`/`TeamDelete`/브로드캐스트/플래그 참조 제거 → 팬아웃을 워크플로우 스크립트로 전환 → 남은 협업을 이름 붙인 에이전트 + `SendMessage`로 재작성 → 일괄 `model: "opus"` 고정 해제. 팩토리가 v1 산출물을 감지하면 이 과정을 자동화합니다 (Phase 0).

## 선행 연구 결과 (v1)

15개 소프트웨어 엔지니어링 과제에 대한 통제 A/B로 구조화된 사전 설정이 LLM 코드 에이전트 출력 품질에 미치는 영향을 측정: 평균 품질 49.5 → 79.3 (+60%), 승률 15/15, 출력 분산 −32% (n=15, 저자 자체 측정, [revfactory/claude-code-harness](https://github.com/revfactory/claude-code-harness) 참조). 저자 측정 수치이므로 도입 결정 시에는 자체 파일럿 측정을 권장합니다.

## 포크 변경사항 — 원본 대비 무엇을 더했나

**원본:** [revfactory/harness](https://github.com/revfactory/harness) v2.1.0 (`main` @ `92d9f1b`, 2026-09-28) · **이 포크:** v2.0.0 · 전체 이력은 [CHANGELOG.md](CHANGELOG.md).

이 포크의 버전은 원본과 따로 센다. 포크 v1.3.0~v1.4.2는 원본 v1.2.0 위에 쌓은 것이고, 포크 v2.0.0은 원본 v2.1.0을 병합한 뒤 그 기능을 v2 구조에 다시 옮긴 것이다. 실행 엔진(3중 실행 모드·품질 패턴·모델 티어·`harness:evolve`)은 원본 그대로다.

### 1. 스펙 추적 (`references/spec-tracking.md` · SKILL.md 0-1, 5-6)

원본에는 실행 이력을 남기는 장치가 없다. 이 포크는 GitHub Spec Kit의 **문서 모델만** 빌려 온다 — 번호 스펙 디렉터리(`NNN-slug/`), `specs/index.md` 대장, `Draft → In Progress → Done → Superseded` 수명주기.

- **5-6** — 새로 만드는 모든 하네스에 스펙을 넣고, 오케스트레이터 템플릿 A·B·C의 시작·마무리 단계에 훅을 둔다. 워크플로 조율 모드에서는 스크립트 안이 아니라 호출 전·반환 후 단계에 둔다(`Date.now()` 금지와 재개 보존 때문).
- **0-1** — 같은 규칙을 하네스 구축 작업 자체에 적용한다. 범위가 모호하면 Draft로 멈춰 합의한다.
- 스펙은 **프로젝트 폴더 최상위 `specs/`**에 두고, 메인 에이전트만 쓴다.

### 2. 명명 규칙 (SKILL.md 2-4)

에이전트 `{harness}-{role}.md`, 진입 스킬 `{harness}/`, 나머지 스킬 `{harness}.{action}/`, 정의 파일은 대문자 `SKILL.md`. 접두사 약어는 금지한다. 프론트매터 `name`을 파일 이름과 다르게 두면 `subagent_type`으로 호출되지 않아 `general-purpose`로 우회하게 되고, 정의 파일의 `model`·`tools`가 적용되지 않는다.

### 3. `CLAUDE.md` 연결 정보 두 곳과 `$HARNESS_ROOT` (SKILL.md 5-4 · `references/claude-md-pointer.md`)

원본은 연결 정보를 구축할 때 한 번만 적는다. 이 포크는 **(a) 하네스 홈**(구축 시 1회)과 **(b) 출력 프로젝트 폴더**(실행마다)로 나누고, (b)를 오케스트레이터 훅으로 넣는다. 원본 위치는 `$HARNESS_ROOT/{harness}/`로 적는다.

### 4. 중간 산출물 폴더: `_workspace/` → `.{harness}/` (SKILL.md 5-1)

공용 `_workspace/` 대신 하네스마다 자기 이름의 숨김 폴더를 쓴다. 원본의 41곳(`evolve`·`execution-modes`·`workflow-recipes` 포함)에 모두 적용했다.

### 5. 7단계 절 번호 (SKILL.md 7-1~7-5)

외부 문서가 "메타스킬 7-3"처럼 참조하므로 원본 7단계에 7-1~7-5 대응표를 붙였다. 상세 진행은 원본의 `harness:evolve`가 맡는다.

### 한눈에 보는 차이 (원본 v2.1.0 대비)

| 파일 | 원본 | 포크 | 변경 |
|------|-----:|-----:|------|
| `skills/harness/SKILL.md` | 405 | 462 | +90 / −33 |
| `references/orchestrator-template.md` | 295 | 319 | +53 / −29 |
| `references/spec-tracking.md` | — | 181 | 신규 |
| `references/claude-md-pointer.md` | — | 49 | 신규 |
| `references/team-examples.md` · `team-patterns.md` · `skill-testing-guide.md` | | | `.{harness}/`·명명 치환만 |
| `references/execution-modes.md` · `workflow-recipes.md` · `skill-writing-guide.md` · `skills/evolve/SKILL.md` | | | 1~2줄 |

**원본에서 뺀 것:** 모션 릴 GIF 2종과 `docs/harness-animation.mp4`(합계 약 20MB). 플러그인을 설치할 때마다 받게 되기 때문이다. 애니메이션은 원본 페이지 링크로 대신한다.

## 라이선스

Apache 2.0
