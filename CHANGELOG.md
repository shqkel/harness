# Changelog

이 프로젝트는 [Semantic Versioning](https://semver.org/)을 따릅니다. **이 포크(shqkel/harness)의 버전은 원본과 따로 센다.** 원본 revfactory/harness의 이력은 아래 「상류 이력」에 `(상류)` 표시로 둔다.

## [Unreleased]

---

## [2.0.0] - 2026-10-08

원본 [revfactory/harness](https://github.com/revfactory/harness) **v2.1.0을 병합**하고, 포크 1.3.0~1.4.2의 기능을 v2 구조에 다시 옮겼다. 포크 커밋은 v1 본문 기준 diff라 줄 단위로 얹지 않고 재작성했다. 이 포크의 버전은 원본과 따로 센다. 원본이 바꾼 내용은 아래 「상류 이력」의 [2.1.0] (상류)·[2.0.0] (상류)를 본다.

### Breaking (원본 v2에서 들어옴)
- `TeamCreate`/`TeamDelete`/`team_name`·`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` 제거 → 워크플로 조율 / 지속형 에이전트 협업 / 서브에이전트 위임 3중 실행 모드
- `model: "opus"` 일괄 지정 폐기 → 업무 특성별 fable/opus/sonnet 선택
- Phase 7 진화 절차 → `harness:evolve` 스킬로 분리 (포크의 `references/harness-evolution.md`는 내용이 모두 겹쳐 삭제)
- `references/agent-design-patterns.md` → `references/team-patterns.md`

### 다시 옮긴 포크 기능
- **중간 산출물 폴더 `.{harness}/`** — 원본 v2의 `_workspace/` 41곳(`evolve`·`execution-modes`·`workflow-recipes` 등 새 파일 포함)을 모두 치환. 5-1에 정의, `execution-modes.md` v1→v2 대응표에 이름 변경 항목 추가
- **스펙 추적** — 0-1(구축 작업 자체), 5-6(생성 하네스 내장), 템플릿 A·B·C·혼합의 시작·마무리 훅, 작성 원칙 10
- **명명 규칙 2-4** — 3·4단계, `team-patterns.md`, `skill-writing-guide.md`, 템플릿 `name`(`{domain}-orchestrator` → `{harness}`)에 반영
- **5-4 연결 정보 두 곳 + `$HARNESS_ROOT`** — 템플릿 훅과 작성 원칙 11. 양식은 `references/claude-md-pointer.md`로 분리
- **7단계 절 번호 7-1~7-5** — 외부 참조("메타스킬 7-3") 보존

### Added (v2에 맞춰 새로 정한 것)
- **워크플로 모드의 스펙 훅 위치** — 스크립트 안이 아니라 호출 전(0단계)·반환 후(정리 단계)에 둔다. 스크립트에서 날짜를 만들면(`Date.now()`) 재개가 깨지므로 실행 로그 날짜는 메인 에이전트가 쓴다. `spec-tracking.md`에 실행 모드별 훅 위치 표 추가
- **스펙은 메인 에이전트만 쓴다** — 여러 에이전트가 같은 `spec.md`를 동시에 고치면 기록이 엇갈린다
- **프론트매터 `name` = 파일 이름** — 다르면 `subagent_type` 호출이 안 돼 `general-purpose`로 우회하고, 정의 파일의 `model`·`tools`가 적용되지 않는다(2-4)

### Changed
- `SKILL.md` 504줄 → 5-4 양식 분리 후 462줄 (500줄 상한 준수)
- manifest를 포크 기준(`owner`·`author`·`homepage`·`repository`)으로 유지하고 description에 「기반: 원본 v2.1.0」 명시
- README 2종(EN/KO) 「포크 변경사항」을 원본 v2.1.0 대비 실측으로 다시 작성, 설치 안내를 `shqkel/harness`로

### Removed
- `harness_reel_en.gif`·`harness_reel_ko.gif`·`docs/harness-animation.mp4`(합계 약 20MB) — 플러그인 설치 때마다 받게 되므로 뺐다. README는 원본 애니메이션 페이지 링크로 대신
- `README_JA.md` — 원본 v2와 같이 삭제

---

## [1.4.2] - 2026-09-22

### Fixed
- **§2-4의 `{harness}/` 조항을 "진입 스킬" 규약으로 정정** — 종전 문구("스킬이 하나뿐인 하네스는 생략 가능, 둘째가 생기면 나눈다")는 스킬이 여럿인 하네스의 진입 스킬까지 개명하라는 뜻으로 읽혔다. 실측(`alter-study`: 진입 `alter-study` + `alter-study.{advisor,editor,librarian,roommate,tutor}` 5종)에서 그 개명은 사용자가 직접 부르는 트리거 이름을 깨뜨릴 뿐 충돌도 식별 저해도 없었다. 접두사가 이미 붙어 있으므로 `{harness}/`를 진입 스킬 자리로 못 박는다

---

## [1.4.1] - 2026-09-22

### Fixed
- **manifest 메타데이터를 포크 기준으로 정정** — `owner`·`author`·`homepage`·`repository`가 포크 이전(revfactory) 값 그대로여서, 이 포크에만 존재하는 1.3.1~1.4.0의 이슈·PR을 upstream으로 보내게 돼 있었다. 실제 설치 소스는 `shqkel/harness`다(`settings.json`의 `extraKnownMarketplaces`·클론 remote 모두 일치). 상류 저작권 표기(`LICENSE`의 `Copyright 2025 robin`, Apache-2.0)는 그대로 둔다 (`.claude-plugin/marketplace.json`, `.claude-plugin/plugin.json`)
- marketplace `description` 신설 — `claude plugin validate`가 경고하던 누락분. 포크 계보를 함께 밝힌다

### Docs
- **README 3종에 「포크 변경사항」 절 신설** — 상류(v1.2.0 `main` @ `cceac68`)와 대조한 실측 차이를 항목별 근거·파일별 증감표와 함께 명시. 상단에 포크 고지 배너 추가. 설치 안내를 `revfactory/harness` → `shqkel/harness`로 정정 — 이 포크 사용자를 upstream으로 보내고 있었다
- CHANGELOG 1.3.2의 「3-0/4-0 중복 검토·재사용 설계 추가」는 상류 현재 `main`에도 존재하므로 포크 차이가 아니다 — README에 단서로 명기

### Known
- README 3종의 배지·star-history 링크는 아직 upstream 저장소를 가리킨다(스타 수 표시 목적)

---

## [1.4.0] - 2026-09-22

### Added
- **명명 규칙 신설 (§2-4)** — 에이전트 `{harness}-{role}.md`, 스킬 `{harness}.{action}/SKILL.md`, frontmatter `name`도 동일 패턴. `.claude/agents/`·`.claude/skills/`는 한 프로젝트의 모든 하네스가 공유하는 네임스페이스인데 규칙이 어디에도 없어, 접두사 누락(`equipment-planner`)·접두사 약어(`cs-writer`)·소문자 `skill.md`가 누적되고 있었다. 스킬 정의 파일명은 대문자 `SKILL.md`로 고정 — 대소문자 무시 파일시스템에서만 통과하던 소문자 이름은 배포 시점에 드러나는 결함이다. 기존 하네스는 일괄 소급하지 않고 다음에 손대는 시점에 맞춘다 (SKILL.md §2-4·§3·§4·§6-1·체크리스트, `references/skill-writing-guide.md`, `references/agent-design-patterns.md`)
- **스펙 우선 — 하네스 구축 자체에 적용 (§0-1)** — 종전 §5-6은 "생성되는 하네스에 스펙 추적을 심어라"였을 뿐, 메타스킬 자신의 구축·수정 작업에는 스펙이 없었다. Phase 0에 스펙 개설/귀속 훅을 넣고, 위치(`$HARNESS_ROOT/specs/` 또는 `{프로젝트}/specs/`)·게이트(신규 구축·아키텍처 변경은 Draft로 합의 후 In Progress)·닫기(Phase 6 통과 후 Done)를 정했다. CLAUDE.md 변경 이력은 *무엇이 바뀌었나*만 남기고 *무엇을 안 하기로 했나*는 남기지 않는다 (SKILL.md §0-1·체크리스트)

### Changed
- **§5-4 CLAUDE.md 포인터를 두 자리로 분리** — (a) 하네스 홈 `{harness}/CLAUDE.md`는 구축 시 1회, (b) 출력 프로젝트 폴더 `{출력 경로}/CLAUDE.md`는 **실행마다**. 종전 §5-4는 "하네스 구축 시 1회"의 절차로만 쓰여 있어, 실행마다 출력 폴더가 달라지는 하네스에서 새 폴더에 포인터가 남지 않았다(rapid-learner 실측). (b)는 절차가 아니라 오케스트레이터 훅으로 박는다 — 시작 Phase에 확인/개설, 최종 보고 Phase에 변경 이력 한 줄 (SKILL.md §5-4, `references/orchestrator-template.md` 템플릿 A·B·C + 작성 원칙 9)
- **원본 포인터에 `$HARNESS_ROOT` 표기 도입** — 사본이 된 `.claude/`가 원본을 되짚을 수 있도록 출력 폴더 CLAUDE.md에 `**원본:** $HARNESS_ROOT/{harness}/`를 적는다. 경로를 리터럴로 박으면 원본이 옮겨지는 순간 거짓이 되므로 환경변수로 가리키고, 미설정이면 추측하지 말고 사용자에게 묻는다 (SKILL.md §5-4)

### Changed (분량)
- **Phase 7(하네스 진화)을 `references/harness-evolution.md`로 분리** — 위 추가분으로 SKILL.md가 538줄이 되어 스킬 자신이 규정한 500줄 상한(§4-4)을 어겼다. Phase 7은 실행 후·유지보수 시점에만 필요한 조건부 내용이므로 분리 1순위다. 본문에는 7-1~7-5 요약표와 "언제 이 파일을 읽으라"는 포인터를 남겼고 절 번호는 그대로 유지했다(외부에서 "메타스킬 7-3"으로 참조 중). 485줄

### Docs
- `references/skill-writing-guide.md` 목차에 §9 누락분 추가

---

## [1.3.3] - 2026-09-01

### Added
- **오케스트레이터 템플릿에 스펙 훅 내장** — 템플릿 A(Phase 0 / Phase 5)·B(Phase 0 / Phase 4)·C(하이브리드)에 "스펙 개설/귀속"과 "완료 기록 → Done → index 갱신"을 명시하고, 작성 원칙 8번으로 승격. 종전에는 SKILL.md §5-6의 산문 지시로만 존재해 하네스 생성자가 훅을 빠뜨리거나 옛 경로로 심어도 걸러지지 않았다 — 1.3.2의 경로 이관이 반쪽으로 끝난 원인과 같은 자리다 (`references/orchestrator-template.md`, SKILL.md §5-6)

---

## [1.3.2] - 2026-09-01

### Changed
- **스펙 디렉토리를 프로젝트 폴더 최상위 `specs/`로 변경** (기존 `.{하네스명}/specs/`) — 한 프로젝트 폴더가 하네스를 여럿 쓸 때 ① 각 하네스가 001부터 세어 번호가 충돌하고 ② 하네스 경계를 넘는 작업이 어느 대장에도 온전히 속하지 못하는 문제가 실제로 발생했다. 대장·번호 수열은 프로젝트당 하나로 두고, 하네스 구분은 대장의 「하네스」 컬럼이 맡는다. 이미 `specs/`가 있으면 다음 번호를 이어 쓴다 (`references/spec-tracking.md`, SKILL.md §5-6, §6-1 검증 항목)

### Added
- 신규 에이전트/스킬 생성 전 중복 검토 단계 (Phase 3-0, Phase 4-0)
- `references/agent-design-patterns.md` "에이전트 재사용 설계" 섹션
- `references/skill-writing-guide.md` §9 "스킬 재사용 설계"

### Changed (재사용 설계)
- Phase 선택 매트릭스에 3-0/4-0 명시
- Phase 2-3에 재사용 검토 단계 포인터 추가
- 산출물 체크리스트에 재사용 검토 항목 2개 추가

---

## [1.3.1] - 2026-08-09

### Docs
- `references/spec-tracking.md`에 "Spec Kit 단계 대응"(constitution~converge ↔ 하네스 Phase 분담표)과 "게이트 규칙"(Draft 합의·Superseded 사전 확인) 추가 — 스펙과 파이프라인의 역할 경계 명문화

## [1.3.0] - 2026-08-09 (shqkel/harness 포크)

### Added
- **스펙 추적 규격 내장** — 신규 하네스 생성 시 `.{하네스명}/specs/` 실행 이력 관리(스펙 문서·index 대장·시작/최종 보고 Phase 훅)를 자동 포함 (SKILL.md §5-6, §6-1 검증 항목, `references/spec-tracking.md` 신규)

### Changed
- **내부 산출물 폴더 관례 `_workspace/` → `.<하네스명>/`** — 하네스별 hidden 폴더로 전환해 여러 하네스가 한 작업 디렉토리에서 충돌 없이 공존 (SKILL.md, orchestrator-template, skill-testing-guide, team-examples, agent-design-patterns)

---

# 상류 이력 — revfactory/harness

포크 2.0.0이 병합한 원본 이력이다(`main` @ `92d9f1b`). 원본 1.2.1 이하는 포크 1.3.0 이전의 공통 이력이다.

## [2.1.0] (상류) - 2026-09-26

### Changed

- **모델 정책 개편: "세션 모델 상속 기본" → "업무 특성 기반 티어 선택"** — 에이전트를 정의할 때 업무의 복잡도·작업 기간·자율성·응답 속도 4가지 기준으로 fable(계획을 세워 장기간 자율 실행하는 최고 난도 업무) / opus(설계·코드 생성·복잡한 분석·교차 검증) / sonnet(로그 분석·형식 변환·단순 수집 같은 일상 업무)을 에이전트별로 고르도록 변경. 애매하면 sonnet, 근거 없는 일괄 지정 금지 원칙 유지
- **스킬 문장 정리** — harness 스킬 본문과 레퍼런스 9종의 번역투를 걷어내고 용어를 통일
- **버전 정합성 2.1.0 동기화** — `.claude-plugin/plugin.json`·`marketplace.json`·README 뱃지(EN/KO)를 2.1.0으로 통일

### Added

- **`references/model-selection-guide.md`** — 티어별 핵심 역할·적합 업무·선택 상황, 티어 구분 기준, 하네스 적용 규칙(업무 단위 판단, 계층 분리, 워크플로 단계별 적용) 상세 가이드
- **생성물 언어 규칙** — 생성하는 에이전트·스킬·오케스트레이터는 스킬 문서의 언어가 아니라 사용자가 대화에 쓰는 언어로 작성 (#28)
- **단계 경계 산출물 동결** — 지속형 에이전트 협업에서 다음 단계를 시작하기 전에 완료 보고를 모두 확인하고, 동결을 알리고, 산출물 해시를 기록해 최종 검증에서 다시 확인 (#53)
- **에이전트·스킬 중복 검토** — 새 에이전트·스킬을 만들기 전에 기존 것과 겹치는지 확인하고, 겹치면 기존 것을 확장. v1.x에 병합된 기여(#17)를 v2 구조로 이식

### Fixed

- **다시 시도해도 소용없는 실패 구분** — 사용량 한도 소진·인증 만료·권한 거부는 재시도하지 않고, 부분 산출물로 진행 정도를 확인해 누락을 파일로 기록하고 보고. 오케스트레이터는 확인한 사실만 대신 반영하고 에이전트의 판단은 추측해 채우지 않음 (#53)
- **`tools:` 지정 시 주의 사항** — 산출물을 고치는 에이전트에 Edit가 빠지는 문제, 지연 로딩 도구가 주입되지 않을 수 있는 문제 (#53)
- **설치 명령 오류** — README·quickstart의 `harness@harness`를 `harness@harness-marketplace`로 수정 (#46)
- **marketplace 소유자 이메일** — v2 브랜치에서 빈 값으로 돌아간 `owner.email`을 main의 값으로 유지

## [2.0.0] (상류) - 2026-07-19

전면 재구축 (ground-up rebuild). v1의 전제였던 실험적 Agent Teams API가 현행 Claude Code에서 사라졌고, 결정적 오케스트레이션을 위한 Workflow 도구가 새로 추가된 환경 변화에 맞춰 모든 것을 다시 설계했다.

### Breaking / Fixed

- **`TeamCreate`/`TeamDelete`/`team_name` 전면 제거** — 현행 런타임에 존재하지 않는 API. 세션의 단일 암묵 팀 + `Agent(name:)` + `SendMessage` 구조로 전 템플릿 재작성. v1 오케스트레이터는 이 API를 호출하다 단일 에이전트 실행으로 조용히 퇴화하는 실질적 브로큰 상태였다
- **`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` 의존 제거** — 플래그 안내 문서(`docs/experimental-dependency.md`) 및 모든 참조 삭제. v2는 플래그 없이 동작한다
- **`model: "opus"` 전 에이전트 강제 정책 폐기** — 세션 모델 상속이 기본. 오버라이드는 근거(기계적 작업의 비용 절감 / 최고 난도 검증)가 있을 때만 명시
- **README-실체 불일치 해소** — v1 README가 홍보하던 `/harness:evolve` 스킬이 실제로는 존재하지 않았다. v2에서 실제 스킬로 출시
- **"세션당 한 팀" 제약 및 팀 재구성 절차 삭제** — 제약 자체가 소멸

### Added

- **3중 실행 모드 체계** — 워크플로우 오케스트레이션(신규) / 퍼시스턴트 에이전트 협업(v1 팀 모드 대체) / 서브에이전트 위임. 모드 선택 기준을 "팀 크기"에서 **"제어 흐름의 결정성"**으로 재정의
- **워크플로우 오케스트레이션 모드** — `Workflow` 도구 기반: `pipeline()`/`parallel()`, 구조화 출력 스키마(파싱 불필요), `budget` 연동 규모 조절, `resumeFromRunId` 부분 재실행, 커스텀 `agentType`, `isolation: worktree`
- **품질 패턴 카탈로그** — 적대적 검증, 관점 분산 검증, 심판 패널, loop-until-dry, 다각 스윕, 완전성 비평가, 침묵 상한 금지
- **`skills/evolve` (`/harness:evolve`)** — 델타 수집 → 피드백 유형 분류 → 일반화 반영 → 변경 이력 갱신 → 진화 보고의 5단계 진화 스킬. 관찰 기반 진화 신호(반복 피드백, 오케스트레이터 우회 흔적) 감지 포함
- **v1 → v2 마이그레이션 경로** — Phase 0에서 v1 산출물 자동 감지, `docs/migration-v1-to-v2.md` + `references/execution-modes.md`에 변환 매핑 표
- **신규 레퍼런스** — `execution-modes.md`(3중 모드 + 마이그레이션), `workflow-recipes.md`(스크립트 스켈레톤 6종 + 함정 12종 체크리스트)
- **데이터 전달 프로토콜에 "구조화 반환" 추가** — 워크플로우 schema 기반 타입 안전 전달을 v2 권장 기본으로
- **워크플로우 기반 A/B 테스트** — 스킬 검증(with/without)을 워크플로우 스크립트로 구성하는 레시피
- **에이전트 정의 확장** — 프론트매터 `tools`(읽기 전용 리뷰어 등 도구 제한), `재호출 지침` 섹션 표준화

### Changed

- **6패턴 각각에 v2 권장 실행 모드 매핑** — 팬아웃/파이프라인은 워크플로우 1순위, 감독자는 퍼시스턴트+태스크, 계층 위임은 워크플로우 1단계 중첩 등
- **오케스트레이터 템플릿 3종 전면 재작성** — A: 워크플로우(정찰→실행→종합, run_meta/resume 포함), B: 퍼시스턴트(스폰+태스크+피드백 루프), C: 서브에이전트
- **Phase 7 재구성** — 운영/유지보수는 harness 스킬에 유지, 진화(피드백 반영)는 evolve 스킬로 분리
- 산출물 체크리스트에 v1 잔재 검증·워크플로우 함정 검증 항목 추가
- `references/agent-design-patterns.md` → `team-patterns.md`로 개편 (품질 패턴 흡수), `team-examples.md` 5종 예시를 v2 문법으로 재작성

### Removed

- **마케팅/사이트 자산 제거** — `harness_banner.png`·`harness_icon.png`·`harness_social.png`·`harness_team.png`(합계 약 9MB), `index.html`(랜딩 페이지), `privacy.html`
- **저장소 운영 부산물 제거** — 런치 캠페인용 `_workspace/` 산출물, 마케팅 에이전트 정의(`.claude/agents/launch-strategist` 등), `docs/experimental-dependency.md`
- README_JA (일본어 README) — 유지보수 부담 대비 효용 저조. EN/KO 2종 유지

## [1.2.1] (상류) - 2026-04-18

### Fixed

- **버전 정합성 동기화** — README.md / README_KO.md / README_JA.md 뱃지가 `v1.0.1`, `.claude-plugin/marketplace.json`이 `1.1.0`, `.claude-plugin/plugin.json`이 `1.2.0`으로 3중 불일치 → 모두 **v1.2.0**으로 통일 (plugin.json 기준)
- **태그드 릴리스 0건 상태 해소 준비** — v1.0.0 / v1.0.1 / v1.1.0 / v1.2.0 소급 태그 계획 작성 (`_workspace/release/audit-2026-04-18.md` §4 참조)

### Added

- **포지셔닝 선언: "harness factory"** — README 상단에 카테고리 자기 규정 문구를 도입. "에이전트 + 스킬을 도메인별로 찍어내는 하네스 팩토리"로 카테고리 선점 (단일 에이전트/프롬프트 프레임워크 대비 차별화)
- **CONTRIBUTING.md** — 기여 가이드 및 SLA 명시 (PR 1차 응답 72h, Issue triage 48h). 커뮤니티 온보딩 장벽 해소
- **docs/ 디렉토리** — 장기 문서(아키텍처, 마이그레이션, 패턴 카탈로그) 이전 공간 신설. README 비대화 방지 및 검색성 향상
- **Issue #3 응답 정책** — 커뮤니티 이슈에 대한 공식 응답 템플릿 및 트리아지 프로세스 추가

### Changed

- `.claude-plugin/marketplace.json` version: `1.1.0` → `1.2.0`
- README 뱃지 (EN/KO/JA 3종): `Version-1.0.1` → `Version-1.2.0`
- **`.claude-plugin/plugin.json` description 재작성** — `"Agent Team & Skill Architect — Meta-skill that designs..."` → `"The team-architecture factory for Claude Code — a meta-skill that turns a domain description into an agent team and the skills they use, with six pre-defined team-architecture patterns..."` (EN+KO 병기, L3 Meta-Factory 포지셔닝 반영)
- **`.claude-plugin/plugin.json` keywords 확장** — 5개 → 17개 (`harness-factory`, `team-architecture-factory`, `claude-code-plugin`, `agent-scaffolding`, `multi-agent`, 6패턴 키워드 6종 추가)

## [1.2.0] (상류) - 2026-04-08

### Changed

- **CLAUDE.md 등록 정책 간소화 (중복 제거)** — Phase 5-4 "컨텍스트 등록"을 "포인터 등록"으로 전환. 에이전트 목록·스킬 목록·디렉토리 구조·실행 규칙 상세를 CLAUDE.md에서 제거하고 **트리거 규칙 + 변경 이력**만 남김. 에이전트/스킬 목록은 `.claude/agents/`, `.claude/skills/` 및 오케스트레이터 스킬에서 단일 출처로 관리
- **Phase 3/4 임시 동기화 단계 삭제** — CLAUDE.md 동기화 부담을 줄이기 위해 Phase 3/4의 임시 동기화 지시 제거. 최종 포인터 등록은 Phase 5-4에서 1회만 수행
- **핵심 원칙 3번 재정의** — "CLAUDE.md에 하네스 컨텍스트를 등록한다" → "CLAUDE.md에 하네스 포인터를 등록한다"
- **CLAUDE.md vs 오케스트레이터 역할 분담표 삭제** — 포인터 정책으로 단순화되어 표 자체가 불필요해짐

### Added

- **Phase 2-1: 하이브리드 실행 모드** — 에이전트 팀 / 서브 에이전트에 더해 Phase별로 모드를 섞는 하이브리드 패턴 추가. 자주 쓰이는 조합(병렬 수집→합의 통합, 팀 생성→검증, Phase 간 팀 재구성) 명시
- **Phase 2-1 실행 모드 비교표** — 팀/서브/하이브리드 3종 특성 및 의사결정 순서 3단계 제공
- **Phase 5-0 하이브리드 오케스트레이터 패턴** — 하이브리드 구성 시 각 Phase 상단에 실행 모드를 명시하는 규칙
- **Phase 5-1 반환값 기반 데이터 전달** — 서브 에이전트 모드 전용 데이터 전달 전략 추가 (기존 메시지/태스크/파일 + 반환값)
- **Phase 5-1 권장 조합 (서브/하이브리드)** — 팀 모드 외 서브 모드와 하이브리드에서의 데이터 전달 권장 조합 명시

## [1.1.0] (상류) - 2026-04-05

### Added

- **Phase 0: 현황 감사** — 트리거 시 기존 하네스 상태를 먼저 확인하고 신규 구축/기존 확장/운영·유지보수 3분기로 라우팅
- **기존 확장 Phase 선택 매트릭스** — 에이전트 추가/스킬 추가/아키텍처 변경별 필요 Phase를 명시한 결정표
- **Phase 3/4 CLAUDE.md 임시 동기화** — 에이전트·스킬 생성 직후 CLAUDE.md에 즉시 반영 (세션 중단 내성)
- **Phase 5-4: CLAUDE.md 하네스 컨텍스트 등록** — 에이전트 팀 구조·스킬 목록·실행 규칙·디렉토리 구조·변경 이력을 기록. CLAUDE.md vs 오케스트레이터 역할 분담표 포함
- **Phase 5-5: 후속 작업 지원** — 오케스트레이터 description에 후속 키워드 필수 포함, Phase 0 컨텍스트 확인 단계로 초기/부분재실행/새실행 자동 판별
- **Phase 5 오케스트레이터 수정 경로** — 기존 확장 시 오케스트레이터를 새로 만들지 않고 수정하는 가이드
- **Phase 7: 하네스 진화 메커니즘** — 실행 후 피드백 수집 → 피드백 유형별 수정 대상 매핑 → 변경 이력 기록 → 자동 진화 트리거
- **Phase 7-5: 운영/유지보수 워크플로우** — 현황 감사→점진적 수정→CLAUDE.md 동기화→변경 검증 4단계
- **description에 운영/유지보수 트리거** — '하네스 점검', '하네스 감사', '하네스 현황', '에이전트/스킬 동기화' 키워드
- **산출물 체크리스트 강화** — CLAUDE.md 동기화 완료, 변경 이력 기록, Phase 0 컨텍스트 확인 항목 추가
- 오케스트레이터 템플릿에 Phase 0 (컨텍스트 확인) 추가 — 에이전트 팀/서브 에이전트 모드 모두 적용
- 오케스트레이터 description 템플릿에 후속 작업 키워드 패턴 포함

### Changed

- 핵심 원칙 2개 → 4개로 확장 (CLAUDE.md 등록, 진화 시스템 추가)
- **"진화 로그" → "변경 이력" 통일** — 이름과 스키마(4컬럼: 날짜/변경내용/대상/사유)를 전 섹션에서 일원화
- **Phase 1 Step 3** — Phase 0 감사 결과를 기반으로 충돌 분석하도록 변경 (중복 제거)
- **5-4 CLAUDE.md 템플릿 코드 블록** — 중첩 렌더링 깨짐 수정 (3백틱→4백틱)
- **역할 분담표 확장** — 스킬 목록, 디렉토리 구조, 변경 이력 행 추가
- **오케스트레이터 템플릿** — Phase 0 컨텍스트 확인 단계, 후속 작업 키워드 가이드 추가

## [1.0.1] (상류) - 2026-03-28

### Changed

- SKILL.md ↔ references 간 중복 내용 제거 (330줄 → 285줄)
  - Phase 2-1: 실행 모드 비교표/불릿 → 핵심 원칙 + agent-design-patterns.md 포인터
  - Phase 2-3: 에이전트 분리 기준 불릿 → 4축 요약 + agent-design-patterns.md 포인터
  - Phase 3: 에이전트 정의 템플릿 코드블록 → 필수 섹션 나열 + references 포인터
  - Phase 5-2: 에러 핸들링 5행 테이블 → 핵심 원칙 + orchestrator-template.md 포인터

## [1.0.0] (상류) - 2026-03-27

### Added

- 6 Phase 워크플로우 기반 하네스 구성 메타 스킬
- 6가지 에이전트 아키텍처 패턴 (파이프라인, 팬아웃/팬인, 전문가 풀, 생성-검증, 감독자, 계층적 위임)
- 에이전트 팀 / 서브 에이전트 실행 모드 지원
- Progressive Disclosure 기반 스킬 생성 가이드
- 오케스트레이터 템플릿 (에이전트 팀 모드 + 서브 에이전트 모드)
- QA 에이전트 통합 가이드 (실제 프로젝트 7개 버그 사례 기반)
- 스킬 테스트/평가 방법론 (With-skill vs Without-skill 비교)
- 실전 팀 구성 예시 5종 (리서치, 소설, 웹툰, 코드리뷰, 마이그레이션)
