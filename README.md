# rnd_proposal

> Multi-step orchestration skill for Korean government R&D proposals.
> 한국 정부 R&D 과제계획서·연구개발계획서·RFP 기반 제안서·공모사업 사업계획서를 단계별 산출물 누적 방식으로 작성하는 [Claude Code](https://docs.claude.com/claude-code) Skill.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skill version](https://img.shields.io/badge/skill-v1.0.0-blue.svg)](#버전)

---

## 무엇을 해결하나

정부 R&D 제안서를 "그럴듯한 문장 묶음"이 아니라 **RFP 요구사항 → 제출 양식 → 평가항목 → 기술개발 흐름 → 기관 역할 → 정량 KPI → 실증·검증 전략**이 맞물린 계획 구조로 설계한다.

연차별 목표·컨소시엄 역할·정량 KPI·실증 범위·인체적용시험·인허가·개별인정·매출 같은 위험 표현은 임의 확정하지 않고 **선택형 인터뷰**(`AskUserQuestion`)로 사용자 결정을 받는다.

## 왜 12단계로 나누는가

단일 거대 프롬프트로 제안서를 뽑으면 다음 문제가 반복된다.

- RFP 필수 요구를 절반만 반영하거나, 양식이 요구하지 않은 내용을 채워 넣는다.
- 연차별 목표·기관 역할·KPI를 LLM이 임의로 확정해 버린다.
- 본문이 표 한두 개와 짧은 단락으로 끝나 평가 분량을 못 채운다.
- 그림·도식이 마지막에 얹히는 장식이 되어 본문 흐름과 어긋난다.

그래서 작업을 12개 역할로 나누고, 각 단계는 **이전 단계가 남긴 구조화 산출물(YAML/표)만** 입력으로 받는다. 단계가 끝나면 결과를 파일로 저장하고, 다음 단계는 그 파일을 다시 읽어 작업한다. LLM이 같은 자료를 매번 새로 해석하지 않고 "앞 단계가 확정한 사실" 위에 쌓는 구조다.

## 작성 흐름

```
입력 분석 (1~4)            →    의사결정 (5~8)              →    작성·검토 (9~12)
─────────────                   ─────────────                    ─────────────
Input Analyzer                  RFP-Form Filling Strategy        Section Writer
RFP Analyzer                    Interview                        Reviewer
Form Analyzer                   Requirement Interview Gate       Visual Asset Planner
Requirement Analyzer            Writing Guide Builder            Export Agent
                                Strategy Planner
```

세 블록의 역할은 다음과 같다.

1. **입력 분석 블록 (1~4단계)** — RFP·양식·요구사항을 구조화 추출. 사용자에게 묻지 않고, 자료에서 뽑을 수 있는 사실만 정리한다.
2. **의사결정 블록 (5~8단계)** — 분석 결과를 근거로 한 선택형 인터뷰. 위험 표현·연차별 목표·KPI 수위·기관 역할·강조점 등 작성 방향을 사용자가 결정한다. **이 블록을 건너뛰면 본문 전체가 LLM의 임의 확정으로 채워진다.**
3. **작성·검토 블록 (9~12단계)** — 결정된 사실 위에 본문을 쓰고, 평가위원 관점으로 검토하고, 시각자료 위치를 본문에 표시하고, 최종 산출물 형식으로 내보낸다.

## 단계별 작성 과정

각 단계의 입력·산출물·사용자 개입 여부를 정리하면 다음과 같다.

| # | 단계 | 입력 | 산출물 | 사용자에게 묻는가 |
|---|------|------|--------|-----|
| 1 | Input Analyzer | RFP, 양식, 참고자료, 평가항목, 참여기관 정보 | `00_inputs/` 복사본, `input_inventory` (자료 목록·누락·가정) | 자료 식별이 모호하면 1회 |
| 2 | RFP Analyzer | RFP 원문 | 사업 목적·핵심 키워드·필수 기술·정량 목표·필수 포함/제외 키워드 (YAML) | 묻지 않음 |
| 3 | Form Analyzer | 제출 양식 | 목차·항목별 작성 목적·표/도식 필요 여부·평가항목 연결 위치·분량 (YAML) | 묻지 않음 |
| 4 | Requirement Analyzer | 1~3 결과 | 기능·시스템·데이터·AI 모델·KPI·검증·실증·역할 분리 요구사항 (YAML) | 묻지 않음 |
| 5 | RFP-Form Filling Strategy Interview | 1~4 결과 | `filling_strategy_questions` + 사용자 결정 누적 | **예 — 핵심 단계** |
| 6 | Requirement Interview Gate | 5 결과 + 미해결 항목 | `risk_decisions.yaml` (연차 목표·기관 역할·KPI 수위·위험 표현 결정) | **예 — 위험 표현 게이트** |
| 7 | Writing Guide Builder | 참고자료, 사용자 답변 | 문체 규칙·논리 흐름·정량 표현 패턴 (Markdown) | 참고자료 없으면 묻지 않음 |
| 8 | Strategy Planner | 1~7 결과 | 핵심 개발 방향·차별화·강조 키워드·우리/타기관 책임 경계·실증 전략 | 경계 모호 시 |
| 9 | Section Writer | 2·4·7·8 결과 (RFP 원문 재해석 금지) | `05_draft/` 본문 Markdown — 기관별 세부 대제목 + 7단 흐름 단락 | 묻지 않음 |
| 10 | Reviewer | 9 결과 + `references/reviewer_checklist.md` | `05_draft/review.md` — 평가항목 대응성·KPI·과장 표현·누락을 `충족/보완 필요/기입 필요`로 정리 | 묻지 않음 |
| 11 | Visual Asset Planner | 9·10 결과 | `06_visual/visual_assets.yaml` + 본문에 `[그림 삽입: VA-001 ...]` 마커 삽입 | 묻지 않음 |
| 12 | Export Agent | 9·10·11 결과 | KPI 표·역할 분담표·연차 계획·HWPX/DOCX 복붙용 Markdown·도식 명세·PPT 구조 | 출력 형식 선택 시 |

핵심은 **5·6단계가 본문 작성 전 게이트**라는 점이다. 두 단계를 건너뛰면 9단계 본문 전체에 임의 확정값이 박혀서 되돌리기 어려워진다.

Section Writer(9단계)에는 별도 규칙이 적용된다. 표 한두 개와 짧은 단락으로 끝내지 않고, 양식이 `[주관연구개발기관]`·`[공동연구개발기관1]`처럼 기관별 기입란을 두면 **기관별 → 세부 대제목(2~4개) → 단락**의 3단 구조로 풀어낸다. 각 세부과업 본문은 `왜 필요한가 → 무엇을 개발하는가 → 어떤 데이터 → 어떤 기술 → 구현 방식 → 검증 방법 → 산출물`의 7단 흐름을 따른다.

## 인터뷰가 작동하는 방식

5·6단계 인터뷰는 일반 챗봇처럼 자유 질문을 던지지 않는다. 다음 규칙으로 동작한다.

- **선택지 기반 순차 질문**: 한 번에 1개(또는 관련 묶음 4개 이내)만 묻는다. 각 질문에 2~4개 대표 선택지를 제공한다.
- **`AskUserQuestion` 도구 우선**: Claude Code의 선택형 UI가 가능하면 텍스트 번호 목록 대신 그것을 쓴다. 직접 입력(`Other`)은 항상 자동 제공된다.
- **추천안 선두 배치**: 분석 결과상 가장 합리적인 안을 첫 번째에 두고 `(추천)` 표시.
- **답변에 따라 다음 질문이 달라진다**: 사용자가 "후보소재 제품화"를 고르면 다음 질문은 인체적용시험·개별인정 책임 범위로, "플랫폼 균형형"을 고르면 플랫폼 기능과 제품화 비중으로 좁혀진다.
- **묻는 범위**: 강조 전략, 사업화 포지션, 연차별 큰 목표, 실증 범위, 컨소시엄 역할 경계, KPI 수위, 제외 범위.
- **묻지 않는 범위**: 모델 아키텍처 파라미터, DB 컬럼 설계, 실험 프로토콜 상세, 예산 세목 — 이 수준은 후속 질문으로 분리한다.

질문 한 개의 내부 형식:

```yaml
question:
  linked_rfp_requirement: "현장 적용 및 Pilot-Test"
  linked_form_section: "1차년도 목표 및 내용"
  why_needed: "1차년도 실증 수위가 자료에 없음"
  question: "1차년도에 Pilot-Test를 실제 수행할지, 실증 설계·예비검증까지 둘지"
  answer_options_or_expected_input:
    - recommended_option: "실증 설계 + 예비검증 (현장 적용은 2차년도)"
    - other_options: ["1차년도 실제 Pilot-Test 수행", "베타 플랫폼만 구축"]
  impact_if_unknown: "1·2차년도 분배가 흔들림, KPI 수치 임의 확정 위험"
```

사용자가 "질문 없이 가정해서 써"라고 명시한 경우에만 임시안을 작성하고, 그때도 본문에 `가정안`·`확정 필요`·`위험 표현`을 표시로 분리한다.

## 중간 산출물이 누적되는 방식

작업 폴더에 단계별 결과가 분리 저장된다. 다음 단계는 이전 단계의 **파일**을 다시 읽어 작업하므로, 같은 자료를 매번 새로 해석할 필요가 없다.

```
proposal_work_<topic>/
├── 00_inputs/              원본 자료 복사본 (수정 안 함)
├── 01_rfp_analysis/        rfp_analysis.yaml              ← 2단계
├── 02_form_analysis/       form_analysis.yaml             ← 3단계
├── 03_mapping/
│   ├── rfp_form_mapping.yaml                              ← 4단계 + 매핑
│   ├── filling_strategy_questions.yaml                    ← 5단계 질문 묶음
│   └── decisions.yaml                                     ← 5·6단계 사용자 결정
├── 04_strategy/            strategy_plan.yaml             ← 8단계
├── 05_draft/
│   ├── draft.md                                           ← 9단계 본문
│   └── review.md                                          ← 10단계 점검
└── 06_visual/              visual_assets.yaml             ← 11단계
```

이 구조가 만들어내는 이점:

- **중단·재개**: 7단계까지 진행 후 중단해도 다음 세션에서 동일 폴더를 읽고 8단계부터 이어간다.
- **부분 재작성**: 9단계 본문 일부만 다시 쓰고 싶으면 1~8단계 결과는 그대로 두고 9단계만 재실행한다.
- **결정의 추적**: `decisions.yaml`만 열면 "왜 1차년도가 예비검증까지로 끝나는지" 같은 질문에 즉답할 수 있다.
- **원본 무수정**: `00_inputs/`의 RFP·양식 원본은 절대 수정하지 않는다. 모든 작업은 복사본 위에서 일어난다.

## 누구를 위한 도구인가

- 정부 R&D 전문기관(KIAT/KEIT/IITP/NRF/NIPA/중기부 등) 공모사업 신청을 준비하는 PM·연구원
- 나라장터 협상에 의한 계약·연구용역에 응찰하는 기관
- HWPX/HWP/PDF/DOCX 양식과 RFP를 함께 다루며 제안서를 작성해야 하는 컨설턴트
- 평가위원 친화 문체·정량 KPI·도식 명세까지 포함한 제안 산출물이 필요한 조직

## 설치

이 스킬은 Claude Code의 user-scope skills 디렉터리에 위치해야 한다.

### 옵션 A. 직접 클론 (권장)

```bash
git clone https://github.com/sangm85/rnd_proposal.git ~/.claude/skills/rnd_proposal
```

### 옵션 B. 특정 버전 클론

```bash
git clone --branch v1.0.0 https://github.com/sangm85/rnd_proposal.git ~/.claude/skills/rnd_proposal
```

### 옵션 C. 개발자용 symlink

저장소를 별도 위치에 두고 스킬 디렉터리에 링크만 연결한다.

```bash
git clone https://github.com/sangm85/rnd_proposal.git ~/dev/rnd-proposal-skill
ln -s ~/dev/rnd-proposal-skill ~/.claude/skills/rnd_proposal
```

설치 후 Claude Code를 재시작하면 `/rnd_proposal` 또는 자연어 트리거(아래 참조)로 자동 활성화된다.

## 사용 방법

### 자동 트리거

다음 키워드가 사용자 메시지에 포함되면 Claude Code가 자동으로 이 스킬을 활성화한다.

- 사업조서, 과제 계획서, 연구개발계획서, R&D 제안서, 정부과제 신청서, 연구계획서, 제안서 작성, 사업계획서, RFP 분석, 공모사업 계획서
- 평가위원 리뷰, KPI 표, 역할 분담표, 연차별 개발 계획, 도식 명세, Mermaid 명세, PPT 변환용 구조, HWPX/DOCX 양식 기준본
- KIAT/KEIT/IITP/NRF/NIPA/중기부 등 R&D 전문기관 공모사업 언급

### 명시 호출

```
/rnd_proposal 사업 제안서 작성
```

### 입력 자료 준비

스킬은 다음 자료가 있을 때 가장 효과적이다.

- RFP / 공고문 (.hwp, .hwpx, .pdf)
- 제출 양식 (.hwpx, .hwp, .docx)
- 참고자료 (과거 제안서, 관련 보고서)
- 평가항목·배점 표 (별표)
- 참여기관·인력 정보 (있는 경우)

자료가 부족해도 진행 가능하나, 임의 확정 위험이 있는 항목은 선택형 인터뷰로 사용자 결정을 받는다.

## 참조 문서

세부 작성 규칙·체크리스트는 `references/` 하위 5개 파일에 위치한다.

| 파일 | 내용 |
|------|------|
| `references/intermediate_schemas.md` | 단계별 중간 산출물 YAML 스키마 |
| `references/writing_rules.md` | 정부과제 문체 규칙·평가위원 친화 표현 |
| `references/section_writer_rules.md` | 본문 작성 원칙 (단락 vs 표·기관별 구분 등) |
| `references/reviewer_checklist.md` | Reviewer 단계 점검 체크리스트 |
| `references/diagram_spec.md` | 시각자산 포맷·렌더링 명세 |

## 함께 쓰면 좋은 스킬

- `hwpxskill` — HWPX(한글) 파일 직접 분석·편집이 필요할 때

## 호환성

- Claude Code v1.x 이상 (Skills 시스템 지원 버전)
- macOS / Linux / Windows (WSL) 동작 확인

## 버전

현재 v1.0.0. 변경 이력은 [CHANGELOG.md](CHANGELOG.md) 참조.

## 라이선스

MIT License — 자유롭게 사용·수정·재배포 가능. 자세한 내용은 [LICENSE](LICENSE) 참조.

## 기여

이슈·PR 환영. 특히 다음 영역의 케이스 공유를 환영한다.

- 새로운 R&D 전문기관 양식·평가항목 패턴
- 다른 도메인(바이오·소재·SW·기계·에너지 등) 작성 사례
- Reviewer 체크리스트 보강

이슈 등록 시 가능하면 다음을 포함해 주세요.

1. 어떤 공고·양식에서 사용했는지 (전문기관·사업명 정도)
2. 어느 단계에서 막혔는지 (Analyzer / Interview / Section Writer / Reviewer)
3. 기대한 결과 vs 실제 결과
