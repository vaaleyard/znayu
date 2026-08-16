---
target: src/routes/+page.svelte
total_score: 24
max_score: 40
na_heuristics:
p0_count: 0
p1_count: 3
timestamp: 2026-08-16T01-36-34Z
slug: src-routes-page-svelte
---
## Design Health Score

| # | Heuristic | Score | Key issue |
|---|-----------|---:|---|
| 1 | Visibility of System Status | 3/4 | Overall state, counts, uptime, incident state, and update time are visible; the rail lacks an explanatory legend. |
| 2 | Match System / Real World | 3/4 | Status terms and dates are clear, but the absolute service count lacks collection context. |
| 3 | User Control and Freedom | 2/4 | Anchor navigation works, but Subscribe is permanently disabled without an in-context explanation. |
| 4 | Consistency and Standards | 3/4 | Semantic colors and repeated status patterns are consistent; historical state is primarily color-coded. |
| 5 | Error Prevention | 2/4 | Static sample timestamps and status values can imply live certainty; sample disclosure is only in the footer. |
| 6 | Recognition Rather Than Recall | 3/4 | Labels are recognizable, but visitors must infer rail semantics and historical/current distinction. |
| 7 | Flexibility and Efficiency | 2/4 | The single read-only path is scannable, but there is no robust day-level inspection or affected-service jump. |
| 8 | Aesthetic and Minimalist Design | 4/4 | Strong hierarchy, disciplined spacing, restrained color, and no ornamental dashboard clutter. |
| 9 | Error Recovery | 1/4 | No stale-data, unavailable-page, or degraded-service communication path is represented. |
| 10 | Help and Documentation | 1/4 | Rail semantics, sample-data status, and subscription availability are unexplained. |
| **Total** | | **24/40** | **Solid visual foundation, moderate trust/support coverage.** |

## Design Specificity Verdict

Znayu feels intentionally authored as a calm operational register: the 800px reading column, continuous service surface, semantic state palette, restrained shadow, and daily uptime rails support the product brief. It remains somewhat category-interchangeable at the brand layer because the lowercase wordmark, native system type, dark-blue palette, green status dot, and uptime bars are familiar status-page conventions. The next opportunity is a restrained, unmistakably Znayu signature that communicates self-hosted ownership without adding decoration.

The target-specific detector ran cleanly with 0 findings and no false positives. Browser visualization was attempted but unavailable, so no overlay or console evidence exists.

## Overall Impression

The page is polished, calm, and unusually disciplined for a status-page prototype. Its biggest weakness is not visual craft but trust framing: authoritative live-status language, dense unexplained uptime rails, and an unfinished-looking disabled action make the evidence feel less reliable than the composition suggests.

## What's Working

1. The masthead answers the top-level availability question before the service register.
2. The continuous register makes four services feel like one operational record rather than four unrelated cards.
3. Emerald, red, amber, and blue are narrowly assigned to operational states, avoiding decorative color noise.

## Priority Issues

### [P1] Prototype data has production-level authority

`Updated 1 minute ago` and `All systems operational` read as live operational claims, while the only sample-data disclosure is in the footer. Move a compact demo/sample disclosure beside the masthead or use explicitly non-production timestamp language. In production, expose freshness and collection state. Suggested command: `$impeccable clarify`.

### [P1] Uptime rails require interpretation and stronger accessibility

The 90/30 colored segments have no nearby legend, and `title` attributes are not a robust keyboard interaction. Add one concise legend explaining day/state semantics and a textual summary; expose day-level details as keyboard-accessible content if inspection matters. Suggested command: `$impeccable audit`.

### [P1] Disabled Subscribe looks unfinished

The visible but permanently disabled control has no local explanation; its description points to a distant footer note. Remove it until supported, or label it as a prototype affordance with visible context. Suggested command: `$impeccable clarify`.

### [P2] Current and historical health are too similar

The masthead says all systems are operational while the Public API rail contains an unavailable historical day. Add explicit current-status versus last-90-days framing and connect the affected rail state to the resolved incident. Suggested command: `$impeccable clarify`.

### [P2] The page is coherent but weakly branded

The visual language could belong to many status products. Add one restrained Znayu-specific signature, such as a register notation or concise ownership/privacy line, without compromising scanability. Suggested command: `$impeccable delight`.

## Persona Red Flags

**Jordan — First-time visitor:** must infer rail semantics; sees an unexplained disabled Subscribe control; may treat the footer sample note as too late; cannot easily connect the red API day to the incident record.

**Alex — Power user / incident checker:** cannot robustly inspect a specific day; must manually distinguish current from historical state; lacks freshness/source/collection-health evidence.

**Maya — Accessibility / low-vision visitor:** color dominates historical state encoding; 90 segments become difficult under zoom; hidden mobile navigation overflow can conceal links; tiny secondary labels may be uncomfortable to read.

## Minor Observations

- The meta description says “Real-time service availability” despite static sample data.
- Service names correctly avoid exposing monitor configuration.
- Smooth scrolling is correctly disabled for reduced-motion users.
- The incident update would be stronger with affected service and start time.
- Full-opacity disabled styling preserves legibility but makes inactivity ambiguous.

## Questions to Consider

- What evidence should a visitor see before trusting “Updated 1 minute ago”?
- Can the uptime rail explain itself in one sentence without becoming dashboard chrome?
- Should Subscribe exist in the prototype at all?
- What is the smallest Znayu-specific detail that would make this unmistakably self-hosted and owned?
