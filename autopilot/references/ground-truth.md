# ground-truth — 起点事实

> 来源：瘦身前 SKILL.md §0（第 369–384 行） 逐字拆出（仅加本文件头两行）。行为以此为准，SKILL 主体只保留指针。

## 0. Ground truth first (silent if missing)

1. `AGENTS.md` / `CLAUDE.md` + `docs/agents/*.md` — tracker, labels, domain
   layout. If absent, default to local-markdown (`.scratch/<slug>/`,
   `spec.md`, `issues/NN-<slug>.md`) + single-context (`GLOSSARY.md` +
   `docs/adr/`).
2. `README.md` / `CONTRIBUTING.md` / `CODING_STANDARDS.md` — run, verify, and
   style commands. The verify command found here becomes the lane gate.
3. `GLOSSARY.md` (+ `GLOSSARY-MAP.md`) + `docs/adr/` — vocabulary; use its
   terms, respect decisions, flag contradictions. If `GLOSSARY.md` is missing,
   do NOT create it eagerly: park new terms in the spec's Glossary section and
   only call `domain-modeling` for genuinely irreversible vocabulary.
4. Domain red-lines before any value/content change (numbers doc, data-vs-code
   source decision, i18n + save-compat). Record the single source of truth
   when a constant is duplicated — and fix every copy (see prelane.md Lane A bar).

## Lane reading list (V2.019: read these, confirm the rest untouched)

Bootstrap reads the full set (§0–§5 split files per SKILL.md). Lane work then
confirms its list before acting; unlisted files stay closed unless the round's
evidence forces them open (record the reason in NOTES):

- **Lane B** — `core-triggers.md` (8 core + grill), `feature-flow.md`,
  `self-answer.md` (STOP list), `verification-gates.md` (red-green + review),
  `conventions.md` (slug/spec shape).
- **Lane C** — `core-triggers.md`, `effort-flow.md`, `self-answer.md`,
  `trace-discipline.md` (map/ticket checkpoints), `conventions.md`.
- **Lane D** — `bug-flow.md`, `core-triggers.md` (incl. mid-lane grill gates),
  `verification-gates.md` (§4 gates + §6 no-harness + §7 closing audits),
  `self-answer.md`.
- **Lane E** — `core-triggers.md` (triage block), `conventions.md` (ticket shape),
  `self-answer.md`.
- **Pure understanding** — ground truth (§0 above) + `self-answer.md` STOP list
  only; no lane files, no marker, no round counted.

