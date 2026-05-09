# Intermediate Schemas

Use these schemas as working memory between agents. Keep unknown values as `[기입 필요]` or `[확인 필요]`; do not invent fixed facts.

```yaml
project_profile:
  사업명:
  지원부처:
  전문기관:
  사업유형:
  과제명:
  개발기간:
  총사업비:
  주관기관:
  참여기관:
  수요기관:
  작성범위:

rfp_analysis:
  사업목적:
  핵심키워드:
  필수기술:
  데이터요구사항:
  실증요구사항:
  정량목표:
  성과요구사항:
  산출물요구사항:
  필수포함키워드:
  제외_주의사항:

form_analysis:
  목차구조:
  항목별작성목적:
  항목별요구내용:
  표필요항목:
  도식필요항목:
  평가항목연결위치:
  분량특성:

rfp_form_mapping:
  - rfp_requirement:
    form_section:
    filling_strategy_to_decide:
    needed_user_decision:
    missing_information:
    draft_risk_if_assumed:
    question_to_user:

filling_strategy_interview:
  must_ask: true | false
  mode: sequential_choice | batch_questions
  questions:
    - form_section:
      linked_rfp_requirement:
      strategy_choice:
      question:
      choices:
        - label:
          description:
          recommended: true | false
        - label: "기타/직접입력"
          description: "사용자가 직접 작성"
          free_text: true
      acceptable_depth: high_level | medium | detailed
      why_needed_before_drafting:
      answer:
      answer_source: choice | free_text
      status: answered | pending

requirements:
  기능요구사항:
  시스템요구사항:
  데이터요구사항:
  AI모델요구사항:
  검증요구사항:
  KPI요구사항:
  산출물요구사항:
  실증요구사항:
  역할분리요구사항:
  사용자확인필요사항:
    연차별목표:
    컨소시엄역할:
    실증범위:
    검증범위:
    KPI확정값:
    제외범위:
    위험표현:

requirement_interview:
  must_ask: true | false
  mode: sequential_choice | batch_questions
  reason:
  questions:
    - id:
      linked_rfp_requirement:
      linked_form_section:
      question:
      why_needed:
      answer_options_or_expected_input:
      choices:
        - label:
          description:
          recommended: true | false
        - label: "기타/직접입력"
          description: "사용자가 직접 작성"
          free_text: true
      impact_if_unknown:
      answer:
      answer_source: choice | free_text
      status: answered | pending
  assumptions_allowed_by_user: true | false
  unresolved_decisions:

writing_guide:
  guide_type: default | reference_based
  문체:
  논리흐름:
  금지표현:
  정량표현규칙:
  평가대응문장패턴:
  기술서술방식:
  표도식스타일:

strategy:
  핵심작성방향:
  핵심개발방향:
  차별화포인트:
  강조키워드:
  평가대응전략:
  우리기관담당범위:
  제외범위:
  실증전략:
  활용_사업화방향:

section_plan:
  section_id:
  section_title:
  목적:
  핵심주장:
  반영요구사항:
  사용할데이터:
  적용기술:
  구현방법:
  검증방법:
  산출물:
  필요한표:
  필요한도식:
  보완필요정보:
```

Reviewer output:

```yaml
review_result:
  overall_status: pass | needs_revision
  findings:
    - item:
      status: 충족 | 보완 필요 | 기입 필요
      reason:
      revision_direction:
  missing_rfp_requirements:
  kpi_gaps:
  role_conflicts:
  overclaim_risks:
  diagram_needs:
```

Visual asset planner output:

```yaml
visual_asset_plan:
  coverage_rule: "section_or_paragraph_level"
  assets:
    - id:
      linked_section:
      linked_paragraph_or_topic:
      insertion_marker:
      asset_type:
      title:
      purpose:
      key_message:
      why_needed:
      components:
      suggested_format:
      render_candidate:
      priority: high | medium | low
  merged_or_skipped:
    - topic:
      reason:
  marker_check:
    total_assets:
    markers_inserted_in_draft:
    missing_markers:
```
