# rnd_proposal

> Multi-step orchestration skill for Korean government R&D proposals.
> 한국 정부 R&D 과제계획서·연구개발계획서·RFP 기반 제안서·공모사업 사업계획서를 단계별 산출물 누적 방식으로 작성하는 [Claude Code](https://docs.claude.com/claude-code) Skill.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skill version](https://img.shields.io/badge/skill-v1.0.0-blue.svg)](#버전)

---

## 무엇을 해결하나

정부 R&D 제안서를 "그럴듯한 문장 묶음"이 아니라 **RFP 요구사항 → 제출 양식 → 평가항목 → 기술개발 흐름 → 기관 역할 → 정량 KPI → 실증·검증 전략**이 맞물린 계획 구조로 설계한다.

단일 거대 프롬프트로 한 번에 본문을 뽑지 않고, 다음 12단계로 나누어 각 단계의 구조화된 결과를 다음 단계 입력으로 사용한다.

```
Input Analyzer → RFP Analyzer → Form Analyzer → Requirement Analyzer
  → RFP-Form Filling Strategy Interview → Requirement Interview Gate
  → Writing Guide Builder → Strategy Planner → Section Writer
  → Reviewer → Visual Asset Planner → Export Agent
```

연차별 목표·컨소시엄 역할·정량 KPI·실증 범위·인체적용시험·인허가·개별인정·매출 같은 위험 표현은 임의 확정하지 않고 **선택형 인터뷰**(`AskUserQuestion`)로 사용자 결정을 받는다.

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

## 산출물 구조

스킬 실행 시 작업 폴더(`proposal_work_<short-topic>/`)에 다음 구조로 중간 산출물이 저장된다.

```
proposal_work_<topic>/
├── 00_inputs/              원본 자료 복사본 (수정 안 함)
├── 01_rfp_analysis/        RFP·과업지시서 구조화 분석 (YAML)
├── 02_form_analysis/       제출 양식 항목별 매핑 (YAML)
├── 03_mapping/             RFP-양식 매핑 + 결정 필요 항목
├── 04_strategy/            전략 계획 (YAML)
├── 05_draft/               본문 초안 (Markdown) + Reviewer 보고서
└── 06_visual/              시각자산 명세 (YAML)
```

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
