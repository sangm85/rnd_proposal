# Visual Asset Spec

Visual Asset Planner does not only create a few representative diagrams. It scans the proposal section by section and creates visual asset specifications wherever a paragraph would be clearer with a figure, mockup, scenario illustration, data example, or diagram.

```yaml
visual_asset_spec:
  id:
  linked_section:
  linked_paragraph_or_topic:
  insertion_marker:
  asset_type:
  title:
  purpose:
  key_message:
  why_needed:
  components:
    - 
  suggested_format: diagram | mockup | table_visual | infographic | screenshot_style | generated_image
  render_candidate: Mermaid | SVG | PPT | Figma | draw.io | imagegen | manual
  visual_style:
    style: modern_government_rnd
    color_tone:
    icon_style:
  notes_for_renderer:
    - PPT 삽입 가능한 구조 사용
    - 화살표 흐름 명확화
    - 텍스트 밀도 최소화
```

## Common Visual Asset Types

- process_flow: data or development process
- system_architecture: platform, model, database, API, service components
- agent_interaction: user, AI agent, tool, database, report, validation lab interactions
- screen_mockup: platform UI, dashboard, report output, evidence package preview
- data_example: sample input/output, scoring result, metadata structure
- validation_result_mock: verification report, assay result summary, model feedback result
- validation_loop: prediction, experiment, feedback, retraining
- role_map: consortium responsibility boundaries
- roadmap: yearly development plan
- kpi_tree: goal, KPI, measurement, evidence
- service_scenario: user journey or deployment scenario
- scenario_illustration: non-diagram illustration showing field use or commercialization scene

## Principles

- Use visual assets to explain technical structure, workflow, user value, validation evidence, or commercialization use. Do not add decoration-only figures.
- Do not cap the number of figures at 3. Generate as many specs as the content needs, but merge redundant visuals.
- For long R&D content, assess visual need at subsection and paragraph-topic level.
- If a paragraph introduces a named module, AI agent, platform function, validation workflow, data package, consortium interface, or commercialization scenario, consider whether a figure should sit directly below that paragraph.
- Every visual asset spec must have an insertion marker that is placed in the draft body at the exact intended location.
- Do not collect all figure markers at the end of a section. Place each marker immediately below the paragraph it supports.
- Prefer input -> processing -> output -> validation -> service linkage.
- Include enough labels for evaluators to understand the development scope.
- Keep visual density low for PPT insertion.
- If Mermaid is requested, generate Mermaid only for suitable diagram types. Use mockup, generated image, or screenshot-style spec when a diagram is not the right format.

## Draft Insertion Marker

Use this marker in the proposal draft:

```markdown
[그림 삽입: VA-001 | 제목: ... | 유형: ... | 목적: ...]
```

The ID must match `visual_asset_spec.id`. If the asset is later rendered, replace or supplement the marker with the rendered figure and caption.
