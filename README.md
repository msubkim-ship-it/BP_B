# BP_B — KPT 교육 리뷰

> **KPT**(Keep · Problem · Try) 방법론을 활용한 **생성형 AI + ECB + Dual-Track TDD** 교육 회고  
> 실습 프로젝트: [UnitConverter_07](https://github.com/msubkim-ship-it/UnitConverter_07)

---

## 1. 교육 개요

| 항목 | 내용 |
|------|------|
| 교육 시간 | 6시간 (Activities 기준) |
| 실습 주제 | Python 길이 단위 변환 CLI — 레거시 → ECB 재구축 |
| 핵심 방법론 | **ECB** 아키텍처 · **Dual-Track TDD** (Logic D-* / UI U-*) · Cursor Agent 인프라 |
| 산출 규모 | 32개 TDD 세션 · pytest **29 passed** (Logic 17 + UI 7 + SSOT 3 + 기타) |
| 회고 프레임 | **KPT** — 잘된 점 유지 · 문제 인식 · 다음 시도 |

### Activities (6시간)

| # | 활동 | 시간 |
|---|------|:----:|
| 1 | 문제 코드 및 기본 요구사항 분석 | 0.5h |
| 2 | 기본·품질 요구사항 구현 (ECB, SRP, 입력 검증) | 2h |
| 3 | TC 구현 (D-* / U-* RED → GREEN) | 0.5h |
| 4 | 추가 요구사항 (설정·동적 등록·출력 포맷) | 2h |
| 5 | 회고 및 발표 (AI 활용·TC·리팩터링) | 1h |

---

## 2. KPT 방법론이란?

애자일·교육 현장에서 널리 쓰이는 **짧은 회고 프레임**이다. 세션·스프린트·교육 일정이 끝난 뒤, 팀이 같은 언어로 배움을 정리한다.

| 구분 | 의미 | 질문 예시 |
|------|------|-----------|
| **K**eep | 유지할 것 | 무엇이 효과적이었는가? 계속할 것은? |
| **P**roblem | 문제점 | 무엇이 막혔는가? 불편·리스크·갭은? |
| **T**ry | 시도할 것 | 다음에 바꿔볼 한 가지는? |

> 본 문서의 KPT는 [UnitConverter_07](https://github.com/msubkim-ship-it/UnitConverter_07) **32개 세션 보고서**(`Report/01`~`32`)와 팀 리뷰를 종합해 정리했다.

---

## 3. K — Keep (유지할 것)

### 3.1 아키텍처·품질 기준

| 항목 | 내용 |
|------|------|
| **ECB 단방향 의존** | `boundary → control → entity` — 레이어 침범을 리뷰·테스트로 조기 차단 |
| **SSOT 원칙** | `entity/constants.py`(수치), `entity/exceptions.py`(E001~E007), `PRD.md`(FR/NFR) — Magic Number·에러 문자열 분산 방지 |
| **OCP/SRP 설계** | `UnitRegistry`, Formatter Strategy — 단위·포맷 추가 시 기존 Service 수정 최소화 |

### 3.2 Dual-Track TDD

| 트랙 | ID | 효과 |
|------|-----|------|
| Logic | D-CONV, D-ERR, D-REG, D-LOC | 도메인·변환·검증을 **Domain Mock 없이** 실제 구현으로 검증 |
| UI | U-CLI, U-FMT, U-CFG, U-IN, U-REG | CLI·포맷·에러 **표면화**를 boundary에서 독립 검증 |
| Golden Master | `tests/boundary/golden/` | UI 출력 회귀 방지 — **Logic Track에는 golden 금지** (리뷰 반영) |

### 3.3 Cursor Agent 인프라

| 계층 | 산출 | 효과 |
|------|------|------|
| Rule | `.cursorrules` | ECB·Dual-Track·TDD 금지(skip/xfail) **헌법** 고정 |
| Skill | `unit-converter-tdd`, `unit-converter-docs` | RED/GREEN/REFACTOR·Export 절차 표준화 |
| Command | `/tdd-session`, `/review-ecb`, `/refactor-safe` 등 | Phase별 **수정 범위** 명시 (red→tests만 등) |
| Hook | `sessionStart` → `session-init.sh` | 세션 시작 시 Rule·Command 컨텍스트 자동 주입 |
| Loop | Test/Review Loop 5기준 | Rule → RED → ECB Review → Skill → pytest 게이트 |

### 3.4 AI 협업에서 잘 작동한 패턴

- **RED 선행 확인** 후 리팩터·스멜 진단 — pytest FAIL/PASS를 Phase 게이트로 사용
- **리뷰 4건 일괄 반영** (API rename, golden boundary 분리, PR Summary, `__init__.py` 금지) — 작은 diff로 계약 정합 회복
- **`/tdd-session <TC-ID>`** 한 줄 호출로 RED→GREEN→Export 일괄 수행 — 교육 중 TC 처리 속도 향상
- **세션 Export** (`Report/`, `Prompting/`) — Transcript·ARRR 보고로 **재현 가능한 학습 기록** 확보
- **팀 리뷰 루프** — 김민주, 김소민, 김연우, 김정균, 김준호 리뷰로 Harness·Golden·네이밍 갭 조기 수정

---

## 4. P — Problem (문제점)

### 4.1 초기 설계·Harness

| 문제 | 영향 | 근거 세션 |
|------|------|-----------|
| `tests/{layer}/__init__.py` 생성 | `import entity`가 `src/entity` 대신 테스트 패키지 로드 → `ModuleNotFoundError` | 04, 07 |
| entity Logic Track에 golden 혼용 | Logic/UI 경계 모호, 승인 기준 불명확 | 06, 07 |
| `int[6]` 1-index 등 레거시 명세 잔존 | UnitConverter 도메인과 불필요한 복잡도 | 01, 02 |
| Harness 골격·TC 0건 상태로 Loop 미폐쇄 | 설계만 있고 pytest 증거 없음 | 01 |

### 4.2 Agent·도구

| 문제 | 영향 |
|------|------|
| Rule / Skill / Command **역할 중복** | RED/GREEN/REFACTOR를 Command로 쪼갤지 Skill만 쓸지 팀마다 해석 상이 |
| GREEN·REFACTOR 전용 Command 부재 (초기) | `/tdd-red`만 있을 때 Phase 호출이 불균형 |
| Hook `additional_context` | Cursor 버전에 따라 주입 실패 가능 |
| **gh CLI 미설치** | PR 본문·원격 작업 수동 복사 (`PR_SUMMARY.md` 의존) |
| 브랜치 `red`/`green` 전환 | working tree 충돌로 일부 세션에서 브랜치 전략 생략 |

### 4.3 구현·리팩터링

| 문제 | 영향 |
|------|------|
| `find_blank_coords` 등 **모호한 API 명** | refactor-safe P0 스멜 — spec 의도와 충돌 |
| `ConversionError` 임시 클래스 등 **이중 에러 모델** | D-LOC-02 GREEN 후 control 연동 시 정리 필요 |
| P1 스멜(conftest SSOT 등) | tests 동결 제약으로 refactor-safe만으로 해결 불가 |
| 레거시 `UnitConverter.py` 병행 | ECB `boundary/cli.py`와 이중 진입점 |

### 4.4 교육 운영

| 문제 | 영향 |
|------|------|
| 6시간 대비 **32세션 분량** | Activities 시간표와 실제 TDD 깊이 간 괴리 — 일정 압박 |
| 문서(Report/Prompting) **대량 누적** | 신규 참여자 온보딩 시 어디부터 읽을지 부담 |
| AI 의존 시 **직접 디버깅 경험** 편차 | pytest FAIL 원인을 스스로 추적하지 않은 경우 학습 깊이 저하 |

---

## 5. T — Try (다음에 시도할 것)

### 5.1 프로세스

| 우선순위 | 시도 | 기대 효과 |
|:--:|------|-----------|
| P0 | 교육 **키트 표준화** — clone 후 `pytest` 1회로 Harness 검증 스크립트 | 04세션형 import 오류 예방 |
| P0 | **KPT를 세션마다** Report §9에 3줄 템플릿 고정 | 회고 누적·발표 자료 자동화 |
| P1 | Hook에 `afterShellExecution` → pytest exit code 게이트 | RED/GREEN Phase **기계적** 준수 |
| P1 | `staging → main` merge 전 **`/review-ecb` 필수** 체크리스트 | ECB 계약 위반 main 유입 방지 |
| P2 | Activities 시간과 **TC 묶음**(예: D-LOC 3건 = 1 Activity) 사전 매핑 | 6시간 교육과 TDD 깊이 정합 |

### 5.2 Agent·문서

| 시도 | 내용 |
|------|------|
| Command 정리 | RED/GREEN/REFACTOR — **Skill 단일 vs Command 3분할** 팀 결정 후 README 한 곳에만 기술 |
| 온보딩 경로 | `Report/README.md`에 **추천 읽기 순서**(01 설계 → 03 RED → 10 tdd-session → 32 완료) 카드 추가 |
| PR 자동화 | CI 또는 `gh` 설치 가이드 + `PR_SUMMARY.md` 템플릿을 교육 사전 과제로 |
| Golden 운영 | U-* GREEN 직후 `/golden-master` **승인 워크플로** (diff 리뷰 → baseline commit) 문서화 |

### 5.3 기술 부채

| 시도 | 내용 |
|------|------|
| `get_g1_ratios_row_major()` SSOT API 확정 + 레거시 별칭 제거 일정 | Mysterious Name 스멜 완전 해소 |
| `conversion_service` ↔ `ErrorCode` 일원화 | D-ERR 묶음과 control 검증 단일 모델 |
| `UnitConverter.py` deprecate 일정 | boundary CLI 단일 진입점 |
| REFACTOR Budget 명시 | 세션당 **파일 수·tests 동결** 규칙을 Skill 상단에 고정 |

---

## 6. 교육 성과 요약

```text
[설계 01] → [Spec/RED 02~03] → [GREEN 04~05] → [REFACTOR 06~07]
    → [GUI/Docs 08~09] → [tdd-session D-* 10~25] → [U-* 26~32]
```

| 지표 | 결과 |
|------|------|
| pytest | **29 passed** (refactoring 브랜치 기준) |
| Logic TC | D-LOC 01~03, D-CONV 01~06, D-ERR 01~06, D-REG 01~02 |
| UI TC | U-IN 01~02, U-CLI 01, U-FMT 01~02, U-CFG 01, U-REG 01 |
| Agent 산출 | rules, 2 Skills, 10+ Commands, Hook, 32 Report, 32 Transcript |
| 리뷰 반영 | API rename, golden 분리, Harness `__init__.py` 금지, PR Summary |

---

## 7. KPT 작성 가이드 (참가자용)

교육 마지막 **1시간 회고·발표**에 아래 템플릿을 사용한다.

```markdown
## KPT — [이름] — [날짜]

### Keep
- (예: `/tdd-session`으로 TC 1건씩 RED→GREEN 완료한 경험)
- (예: entity에 Magic Number 넣지 않고 constants SSOT 유지)

### Problem
- (예: tests/entity/__init__.py 때문에 import 오류 겪음)
- (예: AI가 boundary에 검증 로직을 넣으려 해서 리뷰로 되돌림)

### Try
- (예: 다음 프로젝트에서는 RED FAIL 스크린샷을 세션 보고에 첨부)
- (예: 페어로 `/review-ecb` 표를 함께 읽기)
```

**작성 원칙**

1. Keep/Problem/Try 각 **2~3개** — 많을수록 좋지 않다.
2. Problem은 **비난이 아닌 시스템·프로세스** 관점으로 쓴다.
3. Try는 **다음 스프린트에서 실행 가능한 한 가지 행동**으로 구체화한다.

---

## 8. 관련 링크

| 리소스 | URL |
|--------|-----|
| 실습 저장소 | [github.com/msubkim-ship-it/UnitConverter_07](https://github.com/msubkim-ship-it/UnitConverter_07) |
| 세션 보고서 | [UnitConverter_07/Report](https://github.com/msubkim-ship-it/UnitConverter_07/tree/main/Report) |
| 설계 Transcript | [UnitConverter_07/Prompting](https://github.com/msubkim-ship-it/UnitConverter_07/tree/main/Prompting) |
| PR 본문 템플릿 | [PR_SUMMARY.md](https://github.com/msubkim-ship-it/UnitConverter_07/blob/main/PR_SUMMARY.md) |
| 본 회고 저장소 | [github.com/msubkim-ship-it/BP_B](https://github.com/msubkim-ship-it/BP_B) |

---

## 9. 팀

| 역할 | 이름 |
|------|------|
| 작성자 | 김명섭 |
| 리뷰어 | 김민주, 김소민, 김연우, 김정균, 김준호 |

---

*최종 갱신: 2026-06-05 · UnitConverter_07 세션 32 기준*
