---
name: code-analysis-html
description: Render code/architecture analysis results as a single-file visual HTML dashboard — data-flow diagrams, dependency stacks, stat tiles, CSS charts, and finding cards in the warm-editorial design language. Use when the user asks to visualize an analysis, e.g. "분석해서 시각화해줘", "이미지화해서 보여줘", "다이어그램으로 정리해줘", "이 분석 HTML로 그려줘". Do not apply for prose reports (that is make-html), simple Q&A, or Markdown-only needs.
---

# code-analysis-html — Visual Analysis Design System

A skill for turning **code / architecture / codebase analysis results** into a single self-contained HTML page built around *visual* primitives — pipelines, dependency stacks, stat strips, bar charts, catalog grids, finding cards — instead of prose.

**Visual language:** the same warm-editorial skin as the `make-html` report skill (ivory canvas `#faf9f5`, brown-black ink `#3D3929`, single terracotta clay accent `#D97757`, serif display over Pretendard body) — but with a **dashboard layout**: it opens with the subject's own structure (the pipeline / the stack / the numbers), not with a book-style cover, and every section leads with a diagram or chart.

## make-html vs code-analysis-html — pick the right skill

| | `make-html` | `code-analysis-html` (this skill) |
|---|---|---|
| Content | long-form reading: PRDs, guides, 정책 문서 | analysis results: architecture, usage patterns, codebase surveys |
| Opening | cover (eyebrow + title + meta) | masthead + live subject (pipeline / sparkline / stat strip) |
| Body | prose sections + callouts + tables | diagram-first sections (flow, stack, bars, matrix, chips) + **mandatory program involvement table** |
| Body width | `--max: 800px` reading measure | `1080px` dashboard measure |
| Trigger | "HTML 보고서로", "리포트 형식으로" | "시각화해줘", "이미지화", "다이어그램으로", "분석해서 보여줘" |

## When to Apply — READ FIRST

Apply this design system **only when the user explicitly requests a visual/HTML rendering of an analysis.**

- ✅ Apply: "이 분석 시각화해줘", "이미지화해서 보여줘", "다이어그램/플로우차트로 정리해줘", "이 분석 HTML 대시보드로 만들어줘", or when continuing an earlier visual analysis page.
- ❌ Do not apply: prose report requests (→ `make-html`), simple Q&A, summaries, read-only requests, Markdown/table-only needs, or when another format is specified.
- If ambiguous between report and visual analysis, ask: "산문 리포트(make-html)로 만들까요, 다이어그램 중심의 시각화 페이지로 만들까요?"

## How to Use

1. **Read the full ruleset first.** Before generating, read [references/design-system.md](references/design-system.md) from start to finish. Tokens, the component catalog (flow / stack / stats / bars / matrix / chips / findings), motion recipes, and the responsive rules are all defined there; use only the defined tokens and components.
2. **Reference the example.** Check the finished output shape in [assets/example-influxdb-analysis.html](assets/example-influxdb-analysis.html) — a real backend analysis rendered with the full component set (masthead + sparkline + stat strip → flow → stack → matrix → bars → chip grid → findings).
3. **Output a single HTML file.** Inline the CSS in `<style>` and the motion in a small vanilla `<script>`; no external dependencies except the Pretendard/Noto Serif KR font CDNs. **Never pull in chart/diagram libraries (Mermaid, Chart.js, D3)** — every visual is pure CSS + inline SVG.
4. **Verify with the checklist.** After finishing, go through "10. Authoring Checklist" in design-system.md item by item.

## Core Principles (Summary)

- **Open with the subject, not a hero.** The masthead leads with whatever is most characteristic of the analysis — a data pipeline for a storage system, a module stack for a library, a metric sparkline for a time-series topic.
- **Diagram-first sections.** Every section is `number + serif H2 + lede + one visual`. Prose is limited to the 1–2 sentence lede; the visual carries the content.
- **Always a program involvement table.** Every analysis page must include the program/module table (§7.9) answering *“which programs are related?”* — one row per program, READ/WRITE/CONFIG/INSTALL badges, hot rows highlighted, `—` rows for unrelated programs so coverage is visibly complete.
- **One accent = clay.** Terracotta `#D97757` is the only chromatic accent; it marks the *hot* path (active nodes, primary bars, key numbers). Everything else is the ink family.
- **Alive but editorial.** Animated dash-flow on pipeline arrows, count-up stat numbers, scroll reveals, hover lifts — subtle vanilla motion, never flashy.
- **Numbers must be crisp** — `font-feature-settings: "tnum"` on all numeric text.
- **Self-contained** — single HTML file, works offline except web fonts.

For the detailed rules, tokens, and full markup, see references/design-system.md.
