# BP_B — KPT 팀 회고

> **Cursor AI를 활용한 소프트웨어 개발** 교육을 마친 팀이, **KPT** 방법론으로 각자의 경험을 구조화하고 공유하는 저장소입니다.

---

## 이 저장소의 목적

| ✅ 하는 것 | ❌ 하지 않는 것 |
|-----------|----------------|
| 나의 학습·협업·Agent 활용 경험 **자기 성찰** | 코드·아키텍처·TC 상세 기술 |
| Cursor Agent와 일한 **방식·태도·깨달음** 정리 | 프로젝트 구현 결과 나열 |
| 팀원 각자의 관점 **수집 → 발표용 취합** | 기술 문서·설계서 대체 |

실습은 [UnitConverter_07](https://github.com/msubkim-ship-it/UnitConverter_07)을 매개로 진행되었지만,  
**회고의 주제는 “무엇을 만들었는가”가 아니라 “나는 Agent와 어떻게 학습·개발했는가”** 입니다.

---

## 교육 개요

| 항목 | 내용 |
|------|------|
| 교육명 | **Cursor AI를 활용한 소프트웨어 개발** |
| 실습 | UnitConverter_07 — Agent 인프라(Rule, Skill, Command, Hook)와 방법론 기반 개발 |
| 회고 프레임 | **KPT** (Keep · Problem · Try) |
| 발표 | 개인 회고 취합 → `summary/kpt-team-summary.md` 기반 팀 발표 |

---

## KPT 방법론

짧은 회고 프레임입니다. 발표 전에 각자 같은 구조로 생각을 정리합니다.

| 구분 | 의미 | 본 교육 맥락에서의 질문 |
|------|------|------------------------|
| **K**eep | 유지할 것 | Agent와 함께 **효과적이었던 학습·개발 방식**은? |
| **P**roblem | 문제점 | 막혔던 점, **개념 이해·사람 리뷰·Agent 의존**에서의 어려움은? |
| **T**ry | 시도할 것 | **기본기 스터디·방법론 반복·검증 습관**을 어떻게 바꿀 것인가? |

---

## 회고 작성 가이드

### 초점 영역 (이 중 2~3가지를 골라 쓰면 충분합니다)

- **Agent와 학습** — 모르는 개념을 Agent와 함께 찾아가며 이해한 방식, 학습 파트너로서의 활용
- **방법론·환경 구축** — C2C, Dual-Track TDD, ARRR, Rule/Skill/Command/Hook 등 교육 방법론을 따라가며 작업 환경을 만든 경험
- **개념·기본기** — TDD, RED/GREEN 등 익숙하지 않은 SW 개념·약어를 따라가며 느낀 점
- **spec·설계 리뷰** — 문제 정의, 구조, 원칙을 **사람이 직접 확정·리뷰**해야 하는 순간의 어려움과 판단
- **협업·리뷰** — 팀 협업·PR과 Agent 리뷰 사이에서의 역할 분담, 아쉬웠던 점
- **다음 학습 방향** — Agent 활용과 병행할 기본기 스터디, 반복 학습 계획

### 작성 원칙

1. **1인당 Keep / Problem / Try 각 2~4개** — 항목이 많을수록 좋지 않습니다.
2. **구체적 상황 1~2문장** + **느낀 점·배운 점** — 성과 수치·TC ID 나열보다 **나의 경험**을 씁니다.
3. Problem은 **비난이 아닌 성찰** — “내가 어떤 선택을 했는가, 무엇이 부족했는가”에 초점.
4. Try는 **다음에 실행 가능한 행동** — “더 잘하겠다”보다 “다음엔 ○○할 때 △△하겠다”.

### 작성 예시

아래는 김명섭 회고([reviews/kpt-김명섭.md](./reviews/kpt-김명섭.md))의 톤·구조를 참고한 예시입니다.

```markdown
### Keep
- 익숙하지 않은 개념이 나올 때 Agent에게 질문하며 **이해하면서 진행**하는 방식이 도움이 되었다.
- Rule, Skill, Command, Hook 등을 세팅하며 **요구사항을 만족하는 작업 환경**을 직접 구축할 수 있었다.

### Problem
- TDD, Dual-Track, ARRR 등 **익숙하지 않은 개념·약어**가 많아 실습과 이해를 동시에 따라잡기 어려웠다.
- spec 단계의 문제 정의·구조·원칙을 **사람이 직접 리뷰·확정**하는 데 자신이 없었다.
- 협업·PR 형식이었으나 시간 부족으로 **동료 리뷰보다 Agent 리뷰에 의존**할 수밖에 없었다.

### Try
- TDD·설계 원칙 등 **SW 기본기 스터디를 Agent 활용과 병행**하겠다.
- C2C, Dual-Track TDD, ARRR을 **반복 학습하며 나만의 체크리스트**로 정리하겠다.
- spec 초안을 받은 뒤 구현 전 **스스로 1회 리뷰하는 시간**을 의도적으로 확보하겠다.
```

---

## 제출 워크플로

```text
1. main에서 브랜치 생성
2. reviews/ 아래 본인 KPT md 작성
3. push 후 (선택) PR 또는 팀 채널에 공유
4. 취합 담당자가 main에 팀 요약본 반영 → 발표
```

### 1. 브랜치 생성

```bash
git checkout main
git pull origin main
git checkout -b kpt/이름-영문또는로마자
```

| 팀원 | 브랜치 | 파일명 | 제출 상태 |
|------|--------|--------|:--------:|
| 김명섭 | `kpt/myeongsub-kim` | `reviews/kpt-김명섭.md` | ✅ |
| 김민주 | `kpt/minju-kim` | `reviews/kpt-김민주.md` | ✅ |
| 김소민 | `kpt/somin-kim` | `reviews/kpt-김소민.md` | |
| 김연우 | `kpt/yeonwoo-kim` | `reviews/kpt-김연우.md` | ✅ |
| 김정균 | `kpt/jeonggyun-kim` | `reviews/kpt-김정균.md` | ✅ |
| 김준호 | `kpt/junho-kim` | `reviews/kpt-김준호.md` | ✅ |

> **파일명은 `reviews/kpt-{이름}.md` 형식**을 따릅니다. 작성 시 [KPT_TEMPLATE.md](./KPT_TEMPLATE.md)를 복사해 사용합니다.

### 2. 회고 작성

```bash
cp KPT_TEMPLATE.md reviews/kpt-본인이름.md
```

### 3. 업로드

```bash
git add reviews/kpt-본인이름.md
git commit -m "Add KPT retrospective by {이름}"
git push -u origin kpt/본인브랜치
```

### 4. 취합 (발표 담당: 김명섭)

| 단계 | 담당 | 산출물 |
|------|------|--------|
| 개인 제출 | 각 팀원 | `reviews/kpt-{이름}.md` |
| 취합·요약 | 김명섭 | `summary/kpt-team-summary.md` (main) |
| 발표 | 팀 | 요약본 기반 회고·발표 |

취합 시 **개인 회고 전문을 그대로 붙이지 않고**, 팀 공통 Keep / Problem / Try와 대표 인용을 묶어 발표용으로 정리합니다.

---

## 저장소 구조

```text
BP_B/
├── README.md                 # 본 가이드
├── KPT_TEMPLATE.md           # 개인 회고 작성 템플릿
├── reviews/                  # 팀원 개인 KPT
│   ├── kpt-김민주.md
│   ├── kpt-김연우.md
│   ├── kpt-김정균.md
│   ├── kpt-김준호.md
│   ├── kpt-김명섭.md         # kpt/myeongsub-kim 브랜치
│   └── kpt-{이름}.md
└── summary/                  # 발표용 팀 취합본 (취합 후 main)
    └── kpt-team-summary.md
```

---

## 제출 체크리스트

개인 회고 업로드 전에 확인합니다.

- [ ] `reviews/kpt-{본인이름}.md` 경로·파일명 준수
- [ ] Keep / Problem / Try **모두 작성** (각 2개 이상)
- [ ] **Agent 활용·학습·방법론·리뷰 경험** 중심 (코드·TC 상세 최소화)
- [ ] Try가 **다음 행동**으로 구체적
- [ ] 작성자 실명·작성일·교육명 기재
- [ ] 한 줄 요약 작성 (발표 취합용, 선택 권장)

---

## 관련 링크

| 리소스 | URL |
|--------|-----|
| 실습 프로젝트 (참고용) | [UnitConverter_07](https://github.com/msubkim-ship-it/UnitConverter_07) |
| 본 회고 저장소 | [BP_B](https://github.com/msubkim-ship-it/BP_B) |
| 회고 예시 | [reviews/kpt-김명섭.md](./reviews/kpt-김명섭.md) |

---

## 팀

| 역할 | 이름 |
|------|------|
| 취합·발표 | 김명섭 |
| 회고 참여 | 김민주, 김소민, 김연우, 김정균, 김준호 |

---

*교육: Cursor AI를 활용한 소프트웨어 개발 · 회고 프레임: KPT*
