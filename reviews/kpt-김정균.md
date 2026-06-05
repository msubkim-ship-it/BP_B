# KPT 회고 — 김정균

| 항목 | 내용 |
|------|------|
| 작성자 | 김정균 |
| 작성일 | 2026-06-05 |
| 교육 | Cursor AI 프롬프팅 응용 |
| 실습 | UnitConverter (Cursor AI 활용 TDD) |
| Check-In 컨디션 | 4 / 5 — 팀 PR 리뷰를 여러 건 마무리해서 흐름이 잡혔고, 회고로 정리만 하면 될 것 같아서 |

---

## Keep — 유지할 것
1. Mom Test → `REQUIREMENTS_TRACEABILITY.md` → `ARCHITECTURE.md` 순으로 요구·설계를 먼저 고정한 뒤 TDD에 들어간 흐름이 잘 맞았다.
2. RED에서 모든 테스트를 fail되도록 해놓고 개발을 진행하니 오류가 덜 발생하였다.
3. GREEN은 `pytest.fail` 제거·assert 교체만, skip/xfail로 우회하지 않은 원칙을 지켰다.
4. refactoring은 코드 스멜 우선순위 표를 먼저 작성한 뒤 1스멜=1커밋으로 쪼개서 적용하여 효율적이었다.
5. refactoring 시 1스멜 1커밋·pytest 통과 후 다음 프롬프트 패턴을 유지하여 오류 발생이 적었다.
6. Golden Master `CONV: 2.5,meter,feet=8.2021,yard=2.734025` 불변을 리뷰·본인 작업 모두에서 확인하는 습관을 들였다.

---

## Problem — 문제점

1. `green`을 `staging`에 잘못 merge했다가 revert — 브랜치 역할(red/green/staging/refactoring) 혼선이 생겼다.
2. 본인 refactoring은 Golden Master 스냅샷 없이 smell 진단부터 시작해서 회귀 안전망이 약했다.
3. `git push` SSL 이슈 등 환경 문제로 작업 흐름이 끊긴 적이 있다.

---

## Try — 시도할 것
1. 다음 프로젝트/스프린트 시작 시 **브랜치 역할 표**를 README·Report 1페이지에 먼저 고정하겠다.
2. RED FAIL·GREEN PASS·refactor 직후 pytest 결과를 **세션 보고/Report에 스크린샷 또는 로그 1줄** 첨부하겠다.
4. 리뷰에서 반복된 Problem(`__pycache__`, D-LOC 정의)은 **팀 공유 1-pager**로 정리해 개인 리뷰 부담을 줄이겠다.
5. 다음 회고 때 Try 항목 중 **1개만** Action Item으로 골라 실행 여부를 확인하겠다.

---