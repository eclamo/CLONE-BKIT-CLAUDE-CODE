---
name: sprint-v2122-hardening-progress
description: bkit v2.1.22 Hardening Release 멀티 세션 진행 이력 SoT — 6 sprint(S1 CC v2.1.159 / S2 크로스플랫폼 / S4 데드코드 / S3a god-file분할 / S3b 레이어통합 / S5 최종QA+docs-sync), Kahn order S1→S2→S4→S3a→S3b→S5, 모든 개발 완료 후 릴리즈. 세션 재개 시 본 파일 + master-plan state JSON + /sprint list + Task Management 확인.
metadata:
  type: project
---

# bkit v2.1.22 Hardening Release — 진행 이력 (멀티 세션 SoT)

## 메타
- **브랜치**: `release/v2.1.22-hardening` (main 에서 생성, 2026-06-01)
- **마스터 플랜**: `docs/01-plan/features/v2.1.22-hardening.master-plan.md`
- **State JSON**: `.bkit/state/master-plans/v2.1.22-hardening.json`
- **릴리스 정책**: 6개 sprint **모든 개발/보완/보강 완료 후** v2.1.22 릴리스 (sprint별 부분 릴리스 안 함)
- **Trust**: 대부분 L3, S3a/S3b는 L2(최고위험)
- **ENH 예약**: ENH-324 ~ ENH-360

## 세션 재개 프로토콜 (NEXT SESSION 필독)
1. 본 파일 읽기 → 아래 "진행 현황" 표 확인
2. `.bkit/state/master-plans/v2.1.22-hardening.json` 읽기
3. `/sprint list` + Task Management(`TaskList`)로 다음 unblocked sprint 확인
4. 해당 sprint 작업 (`/sprint start <id>` 또는 phase 진행)
5. **종료 시 반드시**: sprint state 갱신 + 본 파일 "진행 현황" 표 갱신 + master-plan JSON sprints[].status 갱신

## Sprint 의존성 (Kahn order)
```
S1 (독립) ─┐
S2 (독립) ─┤
S4 (독립) ─┼─→ S3a(S4) ─→ S3b(S3a) ─┐
          └──────────────────────────┴─→ S5 (S1+S2+S3b+S4) [LAST]
```
Task 매핑: S1=#6 / S2=#7 / S4=#8 / S3a=#9 / S3b=#10 / S5=#11

## 진행 현황 (세션마다 갱신)
| Sprint | Task | 의존 | Trust | 상태 | 비고 |
|--------|------|------|-------|------|------|
| S1 CC v2.1.159 Response | #6 | — | L3 | ⬜ planned | ENH-317 cancel, monitor 2건, 버전 bump |
| S2 Cross-Platform | #7 | — | L3 | ⬜ planned | process.platform 2→확대, shell-exec 30사이트 |
| S4 Tech-Debt/Dead-Code | #8 | — | L3 | ⬜ planned | pdca-eval stub 6, skip테스트 19파일 |
| S3a God-File Split | #9 | S4 | L2 | 🔒 blocked | 7 god-files ~5899 LOC 분할 |
| S3b Layer Consolidation | #10 | S3a | L2 | 🔒 blocked | 22 subdirs/8-layer 통합 |
| S5 Final QA+i18n+Docs-Sync | #11 | S1,S2,S3b,S4 | L3 | 🔒 blocked | 전체 QA + 8-lang + code=docs |

**현재 위치**: 마스터 플랜 수립 완료, 사용자 승인 대기. 미착수 sprint 0건 시작.

## 핵심 분석 발견 (작업 근거)
- **docs-code-sync CI 맹점 근본원인**: `lib/domain/rules/docs-code-invariants.js`의 `EXPECTED_COUNTS`가 `agents:34` 하드코딩 + lib modules/scripts/subdirs/consecutive 미추적 → 문서 drift 미검출 (S5에서 확장)
- **확정 문서 drift**: README(34 agents/163 modules/51 scripts/94 consecutive/v2.1.139), AI-NATIVE(36 agents/43 skills/142 modules/16 subdirs), README-FULL(34/163/51/94) → 실측 **40 agents/44 skills/188 modules/22 subdirs/v2.1.159/112 consecutive**
- **ENH-317 MOOT**: CC v147 /simplify rename이 v154에서 복원 → bkit /simplify 10 surface valid, deferred 결정 정당화
- **S3 최고위험**: god-file 분할이 226 contract assertion + M1-M10 gate 파괴 risk → L2 + 매 transition 검증
- **S2 corrected**: process.platform 2사이트, raw '/' concat 14(lib), path.join 349, shell-exec ~30
- **S4 corrected**: .skip/.only 19 테스트파일(491은 raw match), pdca-eval stub 6 (contract L4 governance 확인 필요)

## Related memories
- [[cc-version-history-v2147-v2159]] — S1 근거 (CC v2.1.159 분석)
