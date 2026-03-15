# Changelog

## [v2.0.0] - 2026-03-15

### 핵심 변경
- **ralph.sh v2.0.0**: 전면 리팩토링
  - fix_plan.md 읽기 전용 (루프 중 수정 금지)
  - completed_wis.txt = 단일 진실 원천 (SSOT)
  - safe_sync_main: 로컬 main에 커밋 없음 → reset --hard 안전
  - reconcile_fix_plan: 루프 종료 시 fix_plan 일괄 동기화 → 단일 PR
  - recover_completed_from_history: crash 후 git log에서 자동 복구
  - cleanup_stale_completed: PR 충돌 close 시 자동 재실행
  - resolve_conflicting_prs: 충돌 PR 자동 rebase → 실패 시 close + 재실행

### CI gate 강화
- TDD 강제 (PROMPT.md: RED → GREEN → 커밋)
- e2e.yml: 머지 후 Playwright 실행 → 실패 시 regression issue 자동 생성
- smoke 테스트: /wi:start Phase 5.5에서 도메인별 자동 생성
- WI-NNN-1 서브넘버링: regression fix WI 자동 추가

### GH Issue Regression
- CI/e2e 실패 → `gh issue create --label regression`
- inject_regression_wis: open issue → fix_plan에 WI-NNN-1-fix 추가
- closed issue → guardrails.md RAG 흡수

### Merge Queue
- 조직 계정: merge queue ruleset 자동 설정 (/wi:init)
- 개인 계정: strict: false fallback
- enqueuePullRequest GraphQL 연동
- enqueue-pr.sh 래퍼 스크립트

### 신규 스킬
- `/wi:env`: 인프라 환경 구성 (DB, 배포, Secrets 등록)

### 신규 스크립트
- `.ralph/scripts/enqueue-pr.sh`: merge queue PR 등록
- `.ralph/scripts/launch-loop.sh`: 새 터미널에서 루프 실행

### 운영 규칙
- `.claude/rules/ralph-operations.md`: 모든 세션에 자동 적용
  - fix_plan 수정 금지, enqueue-pr.sh 사용, completed_wis SSOT 등

### 도메인 분리 분석
- /wi:start에서 WI 수 + L1 도메인 분리 분석 → 병렬/순차 자동 권장

### 템플릿 복사 방식 변경
- init 스킬: ralph.sh 직접 생성 → `~/.claude/templates/ralph/`에서 복사
- 모든 프로젝트에서 동일한 v2.0.0 보장

### 기타
- commit-msg hook: WI-NNN-N-fix 서브넘버링 허용
- commit-check.yml: 동일 패턴
- pre-push hook: push 대상 ref 기반 판단 + merge queue 브랜치 예외
- .gitignore: completed_wis.txt, loop_state.json 추가
- MAX_TURNS: 25 → 40
- 429 false positive 수정 ("status":\s*429 패턴)

### 검증
- wi-test (FlowHR): 99 WI 완료, 중복 0회, fix_plan 충돌 0개
- MakeLanding: 소규모 프로젝트 테스트

---

## [v1.0.0] - 2026-03-14

### 초기 릴리즈
- Ralph Loop 기본 구조 (순차 + 병렬 worktree)
- RAG 시스템 (codebase-map, wi-history, patterns, guardrails)
- WI 기반 자동 개발 루프
- /wi:init, /wi:prd, /wi:start, /wi:status, /wi:guide, /wi:note 스킬
- CI/CD (lint, build, test, commit-check)
- Git hooks (commit-msg, pre-push)
