# agent-skills

[English](README.md) | **한국어**

기획, 구현, 리뷰를 다루는 Claude Code용 개인 엔지니어링 스킬 모음입니다. 세 컬렉션의 장점을 모아 정리했습니다.

- [cursor/plugins `pstack`](https://github.com/cursor/plugins/tree/main/pstack): 근거의 엄밀함 (3단계 판정, 증명 사다리, 리뷰 lead judgment)
- [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills): 스킬 구조 (Red Flags, 합리화 vs 현실 표, 검증 체크리스트), doubt 기반 리뷰, 품질 기준 지키기
- [mattpocock/skills](https://github.com/mattpocock/skills): 짧고 날카로운 워크플로우 (grilling 라운드, red-capable 디버그 루프, 테스트 seam, 세로 슬라이스, 2축 리뷰)

## 스킬

| 단계 | 스킬 | 호출 주체 | 하는 일 |
|---|---|---|---|
| 라우터 | `using-agent-skills` | 모델 | 작업에 맞는 스킬 선택, 공통 운영 규칙 |
| 기획 | `grill` | 사용자 | 추천 답을 붙인 질문 라운드로 목표가 확정될 때까지 인터뷰 |
| 기획 | `grill-with-docs` | 사용자 | `grill` + `domain-modeling`: 인터뷰하면서 GLOSSARY.md와 ADR도 기록 |
| 기획 | `domain-modeling` | 모델 | 용어 검증, 시나리오로 경계 확인, GLOSSARY.md와 ADR 최신 유지 |
| 기획 | `prototype` | 사용자 | 설계 질문 하나를 해결하는 버릴 코드: 로직 데모, UI 시안, 스파이크 |
| 기획 | `spec` | 사용자 | 테스트 가능한 스펙: 반증 가능한 완료 조건, seam, Always/Ask/Never, 범위 밖 |
| 기획 | `architect` | 사용자 | 타입·모듈 경계부터 설계 (후보 2개 이상, 위험 신호 검사) 후 뼈대에 맞춰 구현 |
| 기획 | `slice` | 사용자 | 세로 슬라이스 티켓, 하나당 컨텍스트 창 하나, 위험한 것부터 |
| 구현 | `tdd` | 모델 | 합의된 seam에서 레드-그린, 품질 기준 지키기 |
| 구현 | `debug` | 모델 | red-capable 루프 게이트, 반증 가능한 가설, 전제 의심 |
| 구현 | `principles` | 모델 | 도메인 모델링, 근본 원인 수정, 빼고 나서 더하기, 두 번 설계, 도구 만들기 |
| 리뷰 | `review` | 사용자 | 의도 고정, 격리된 표준·스펙 리뷰어, mutation 체크, lead judgment |
| 리뷰 | `doubt` | 모델 | 새 컨텍스트에서 주장을 적대적으로 검증. VERIFIED / NOT VERIFIED / INCONCLUSIVE |
| 리뷰 | `blast-radius` | 사용자 | 변경이 안전한 근거 하나를 실제 코드 실행으로 증명 |

일반적인 흐름: `grill -> (prototype) -> spec -> (architect) -> slice -> tdd (슬라이스마다) -> review`

## 출처

각 스킬의 아이디어를 어디서 가져왔는지 정리했습니다. pstack = cursor/plugins `pstack`, addy = addyosmani/agent-skills, matt = mattpocock/skills.

| 스킬 | 주 기반 | 함께 가져온 것 |
|---|---|---|
| `using-agent-skills` | addy `using-agent-skills` (라우터, 핵심 행동 규칙) | pstack `poteto-mode` (주장 근거 라벨, 되돌릴 수 없는 작업 전 멈춤); matt `ask-matt/PHASE-BOUNDARIES` (컨텍스트 경계) |
| `grill` | matt `grilling` (설계 트리, 프론티어 라운드, 추천 답) | addy `interview-me` (가설 + 확신도, "실제로 원하는 것", 명시적 승인, 다음 3개 답 예측 중단 테스트); pstack `sharpen` (네 가지 질문) |
| `grill-with-docs` | matt `grill-with-docs` (grilling + domain-modeling 조합) | — |
| `domain-modeling` | matt `domain-modeling` (용어 검증, 시나리오, 코드 대조, 즉시 용어집 갱신, ADR 3조건, GLOSSARY/ADR 형식) | addy `documentation-and-adrs` (ADR 대상 기준); pstack `principle-model-the-domain` (도메인의 언어 사용) |
| `prototype` | matt `prototype` (로직 HTML 데모, UI `?variant=` 전환기, 버릴 코드 규칙, 브랜치에 보존) | pstack `poteto-mode/playbooks/prototype` (결정 없으면 프로토타입 없음, 스파이크 갈래, 맞는 화면에서 관찰, 추천 보고); addy `frontend-ui-engineering` ("프로토타입은 프로덕션이 된다" → 승격 금지, tdd로 재작성); pstack `benchmark-checklist` (스파이크 측정) |
| `spec` | addy `spec-driven-development` (측정 가능한 목표, Always/Ask/Never, STOP 게이트) | matt `to-spec` (테스트 seam, 파일 경로 금지); pstack `figure-it-out` (반증 가능한 완료 조건, 가장 위험한 미지수 먼저) |
| `architect` | pstack `architect` (기존 시스템 파악, 두 번 설계, 위험 신호 검사, 사용 예시 먼저, 인터페이스 깊이, 폐기 신호, 근거 템플릿) | matt `codebase-design` (두 번 설계 조건을 병렬 서브에이전트로) |
| `slice` | matt `to-tickets` (트레이서 불릿 슬라이스, 차단 관계, 사전 리팩터, expand-contract) | addy `planning-and-task-breakdown` ("and" 분할 규칙, 체크포인트, 미완료 계획 덮어쓰기 금지) |
| `tdd` | matt `tdd` (합의된 seam, 동어반복·구현 결합 안티패턴) | addy `test-driven-development` (스택 탐지, prove-it, 첫 실행 통과 의심), `constraint-driven-development` (품질 기준 지키기), `incremental-implementation` (~100줄 규칙) |
| `debug` | matt `diagnosing-bugs` (red-capable 루프 게이트, 최소화, 가설 3~5개, 디버그 태그) | pstack `principle-attack-the-premise` (수정 2회 실패), `principle-fix-root-causes` (가드 판별, 상시 신호) |
| `principles` | pstack `principle-*` (파일 하나에 규칙 하나, 바꾼 결정 명시, build-the-lever) | matt `codebase-design` (삭제 테스트, 어댑터 1개 vs 2개, 두 번 설계); addy `code-simplification` / `code-review-and-quality` (체스터턴의 울타리, 개념 수 세기) |
| `review` | pstack `interrogate` (의도 고정, 루브릭 관점, lead judgment, 조치/고려/기록/기각) | matt `code-review` (격리된 2축, 세 점 diff, 일찍 실패); addy `code-review-and-quality` (mutation 체크, 코드 건강도 기준 승인) |
| `doubt` | addy `doubt-driven-development` (주장 숨기기, 순서 있는 분류, 3회 상한, doubt theater) | pstack `figure-it-out` / `swarm` (VERIFIED / NOT VERIFIED / INCONCLUSIVE, 공백은 통과가 아님), `blast-radius` (증명 사다리) |
| `blast-radius` | pstack `blast-radius` (안전 근거 하나, 증명 사다리, 보고 형식) | pstack `why` (못 찾은 것도 결과, 호출자 지어내지 않기) |

## 설치

```
/plugin marketplace add /path/to/agent-skills
/plugin install agent-skills@agent-skills
```

스킬은 `/agent-skills:<name>` 형태로 나타납니다.

## 규칙

- `SKILL.md`는 약 150줄 이하로 유지하고, 세부 내용은 필요할 때만 읽는 `references/`에 둡니다.
- description에는 스킬이 무엇이고 언제 쓰는지만 쓰고, 작업 단계는 쓰지 않습니다.
- 사용자가 호출하는 오케스트레이터는 `disable-model-invocation: true`를 설정하고, 규율 스킬은 모델이 호출합니다.
- 스킬 본문은 영어로 쓰고, 스킬은 사용자의 언어로 대화합니다.

## 번역

영어 `README.md`가 원본입니다. 번역본은 같은 위치에 ISO 639-1 코드로 `README.<lang>.md` 이름을 붙여 둡니다 (`README.ko.md`, `README.ja.md`, ...).

언어를 추가하려면:
1. `README.md`를 `README.<lang>.md`로 복사해 번역합니다. 스킬 이름, 파일 경로, 명령어, 코드 블록은 그대로 둡니다.
2. **모든** README 맨 위 언어 전환 줄에 같은 순서로 새 언어를 추가합니다.
3. `README.md`가 바뀌면 같은 커밋에서 각 번역본도 갱신하거나, 번역본 맨 위에 최신이 아니라고 표시합니다.
