# Section Writer Rules

## Non-Negotiable Rule

Do not default to a table like `연구내용 | 수행방법 | 산출물` for technical development sections.

Technical explanation = prose-centered.
Management information = table-centered.

If `1차년도 목표 및 내용`, `2차년도 목표 및 내용`, `연구개발 내용 및 범위`, or `추진전략 및 체계` is only a few sentences plus a table, treat the section as incomplete and rewrite it.

If a form has `(3) 개발내용 및 범위`, this subsection must be much more detailed than the yearly goal summary. When the form provides institution-specific slots such as `[주관연구개발기관]`, `[공동연구개발기관1]`, and `[공동연구개발기관2]`, preserve that institution-by-institution structure. Under each institution, use major work-package headings and substantial prose under each heading.

## Required Flow

For R&D content, include this flow naturally:

```text
왜 필요한가
-> 무엇을 개발하는가
-> 어떤 데이터를 사용하는가
-> 어떤 기술을 적용하는가
-> 어떤 방식으로 구현하는가
-> 어떻게 검증하는가
-> 어떤 결과물이 생성되는가
```

## Yearly Goal Sections

For each yearly goal section, write in prose first. Include these paragraphs before any table:

1. 연차의 전략적 위치: why this year exists in the full project.
2. 연차 목표: what must be achieved by the end of the year.
3. 개발 내용: what will be built, integrated, or refined.
4. 데이터·기술 사용: what data, model, system, or validation method is used.
5. 기관별 수행: who does what, including interfaces between consortium members.
6. 검증·실증: how outputs will be tested, validated, or accepted.
7. 산출물 연결: what deliverables are produced and how they feed the next year or commercialization.

Use a table only after the prose to summarize work packages, milestones, KPIs, roles, or deliverables.

## Development Content And Scope

For `(3) 1차년도 개발내용 및 범위` and `(3) 2차년도 개발내용 및 범위`, use the common Korean government R&D proposal pattern. If the form asks for institution-specific entries, preserve the institution slots and place structured work-package headings under each institution. The numbering style is not fixed; follow the form's existing style (`1), 2), 3)`, `(1), (2), (3)`, `가), 나), 다)`, `①, ②, ③`, or bullets).

Example when the form uses `1), 2), 3)`:

```text
(3) 1차년도 개발내용 및 범위
  [주관연구개발기관(기관명)]
    1) 공공 DB 수집 및 파이프라인 구축
    2) 기능성 후보소재 AI 평가모델 개발
    3) 후보소재 스크리닝 및 우선순위화
  [공동연구개발기관1(기관명)]
    1) in vitro 검증 프로토콜 구축
    2) 후보소재 효능·안전성 예비검증
    3) 검증 데이터 표준화 및 플랫폼 환류
  [공동연구개발기관2(기관명)]
    1) 원료 표준화 및 제품화 자료 구조 설계
    2) 인체적용시험 준비항목 정의
    3) 개별인정 신청자료 패키지 항목 매핑

(3) 2차년도 개발내용 및 범위
  [주관연구개발기관(기관명)]
    1) 실증 데이터 기반 AI 모델 고도화
    2) 플랫폼 실증 운영 및 고객검증
    3) B2B 서비스 모델 정리
  [공동연구개발기관1(기관명)]
    1) 최종 후보소재 심화 검증
    2) 독성·안전성 평가
    3) 검증 리포트 및 환류 데이터셋 제공
  [공동연구개발기관2(기관명)]
    1) IRB 프로토콜 및 시험기관 연계 준비
    2) 개별인정 신청자료 패키지 작성·구비
    3) 제품화·인허가 연계 전략 수립
```

Adjust the headings to the RFP and project domain. The exact headings above are examples, not fixed labels.

Under each major heading, write enough prose to be submission-ready:

1. 목적: why this work package is needed in that year.
2. 세부 개발내용: what will be built, collected, integrated, analyzed, or prepared.
3. 입력과 방법: data, samples, system components, model method, experiment method, or workflow used.
4. 기관 역할: which institution owns the task and how other institutions interface.
5. 검증과 산출물: how completion is verified and what concrete output remains.

Each institution should normally contain 2-4 major headings, and each major heading should contain 2-4 paragraphs or equivalent dense bullets. If each institution is only a short responsibility sentence, treat it as underwritten and expand it before finalizing.

Use a final role table only as a summary. It must not replace institution-specific prose under the form's required institution slots.

### Visual Marker Rule For Development Content

For each major work-package heading under `개발내용 및 범위`, decide whether one image/diagram would help an evaluator understand the work. When useful, place a Visual Asset marker immediately after that heading's prose, not at the end of the year section.

Good pairings:

- `공공 DB 수집 및 AI 학습용 데이터 파이프라인 구축`: data pipeline diagram showing source DBs, cleaning, entity mapping, feature extraction, training dataset.
- `기능성 후보소재 AI 평가모델 개발`: model workflow diagram showing input features, model scoring, evidence ranking, output report.
- `후보소재 스크리닝 및 우선순위화`: funnel diagram from 20-25 candidates to final 3 candidates.
- `in vitro 검증 프로토콜 구축`: validation protocol diagram showing sample, cell model, biomarker, repeat test, 판정기준.
- `원료 표준화 및 제품화 자료 구조 설계`: package/table visual showing raw material, specification, manufacturing, safety, efficacy, package readiness.
- `IRB 프로토콜 및 시험기관 연계 준비`: process diagram showing protocol draft, site consultation, endpoint selection, IRB-ready package.

Do not add decorative images. The visual must explain a workflow, architecture, data example, validation scenario, role map, screen mockup, or output/report example.

## Style

- Use explanatory paragraphs.
- Emphasize technical flow and implementation logic.
- Include data usage, model or algorithm role, system implementation method, validation, and service use.
- Make feasibility visible through concrete inputs, processing steps, outputs, milestones, and test methods.
- Keep tables for yearly schedules, KPI, roles, deliverables, comparisons, and quantitative targets.

## Recommended Section Patterns

Need:
problem -> policy/market/technology evidence -> limitation of current approach -> necessity of this project.

Goal:
final goal -> quantitative targets -> qualitative goals -> yearly goals.

Development content:
development target -> input data -> processing/modeling -> system implementation -> validation -> deliverable.

Differentiation:
baseline or competing approach -> limitation -> proposed difference -> measurable advantage -> validation plan.

Implementation strategy:
work packages -> institution roles -> interfaces -> risk and mitigation -> milestones.

KPI:
baseline -> target -> measurement method -> verification body -> evidence document.

Commercialization:
target user -> use scenario -> adoption channel -> revenue or diffusion assumption -> follow-up scale-up.
