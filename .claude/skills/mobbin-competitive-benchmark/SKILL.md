---
name: mobbin-competitive-benchmark
description: Use when a designer or PM asks to benchmark, compare or audit competitor apps or flows (booking, checkout, onboarding, sign-up, payments) or wants a competitor matrix or scoring sheet. Builds a scored Excel workbook from Mobbin flows, with evidence links and self-test gaps marked. Triggers — "benchmark competitors", "competitive analysis of checkout", "how does Agoda handle booking". Not for comparing a live page to its Figma design (use figma-web-qa-diff) or reviewing a PRD (use prd-review).
version: '1.0.0'
recommended-model: claude-sonnet-5
argument-hint: "[journey] [competitors] — e.g. hotel booking, Booking.com Agoda Airbnb"
allowed-tools: Read, Write, AskUserQuestion, Bash(python3:*), Bash(soffice:*), mcp__mobbin__*, mcp__claude_ai_Mobbin__*, mcp__claude_ai_Google_Drive__*
---


# Mobbin Competitive Benchmark

Turn Mobbin's recorded app flows into a desk benchmark that a design team can act on: what each competitor does at every funnel stage, a 1–5 score per stage, the patterns worth copying or avoiding, and a link to the evidence for every claim.

The output replaces most of a manual "go and book on every app" exercise, but not all of it. Mobbin shows layout, copy, price presentation, steps and options at the moment of capture. It can't show speed, error handling, logged-out paths it didn't record, or other markets. The skill's value depends on being honest about that line, so the workbook always marks what still needs self-testing.

## Workflow

### 1. Scope the benchmark

Work out, from the conversation or by asking one short question if truly missing:

- **Journey**: the flow being benchmarked (e.g. hotel booking, food checkout, e-wallet top-up).
- **Our context**: the user's product, B2C or B2B, and target market (e.g. Indonesia). Market matters because local patterns (payment methods, ID rules, language) often decide the result.
- **Competitors**: the list the user gave, or propose 5–7 with a varied mix of roles: global leader, regional/local player, direct brand, alternative model.
- **Reference template**: if the user shares a benchmarking kit or sheet, read it and mirror its stages, criteria, scoring scale and tabs. The Google Drive connector's CSV export only returns the first sheet; if you can't read the criteria rows, say so and ask for an .xlsx upload, or rebuild criteria from the kit's stated method and flag that in the Read Me.

Default to iOS (mobile is where most abandonment happens) unless the user's product is web-first.

### 2. Check Mobbin coverage first

Before planning criteria in detail, run one `search_flows` per competitor with the app name plus the journey, e.g. `"Agoda booking a hotel room from search results to checkout and payment"`, `limit` 2–3, `output_destination: "doc"`. Keep `task_intent` identical across calls.

If the results come back for a different app, that competitor isn't on Mobbin. Then:
- keep it as a column marked `on_mobbin: false` (it becomes a yellow self-test column), and
- suggest a stand-in that plays the same role (e.g. Trip.com for a missing regional OTA), and say so in the Read Me.

Read `references/mobbin-playbook.md` for query patterns, how to fill missing stages, and how to find local-market patterns.

### 3. Fill the stages

Map each competitor's flows onto the funnel stages. Where a stage is missing (often search/results or payment), run a targeted `search_flows` or `search_screens` for that stage only. Aim for roughly 10–15 Mobbin calls in total; past that, returns diminish and the context fills with images.

Always judge from the screenshots, not the flow names or metadata. While looking, note the **capture context** visible on screen: market, currency, dates shown, logged-in or not. This explains many differences (e.g. a US capture won't show Indonesian e-wallets).

### 4. Define criteria

Use 5–7 funnel stages with 4–6 criteria each (roughly 25–40 total). For each criterion record what to look for and the **driver**: the user problem or business metric it protects (for checkout flows, use the Baymard abandonment drivers in `references/criteria-library.md`). Criteria tied to a driver make the scores defensible in a review.

`references/criteria-library.md` has a ready hotel/travel booking set plus guidance for adapting to other journeys. Set `evidence` to `"Mobbin + Self-test"` for criteria Mobbin can only partly answer (guest checkout, local payments, error states).

### 5. Write observations

One cell per platform per criterion. Good observations are:
- **specific and factual**: describe what is on screen, quoting short UI copy in quotes ('You will pay SGD0 today').
- **short**: one or two sentences.
- **honest about gaps**: if the flow didn't show that moment, omit the platform from `observations` so the cell reads "Not captured". Never infer that a feature is absent just because it wasn't captured.

List platforms with clear friction in that criterion under `friction` so their cells turn red.

### 6. Score

Score each stage 1–5 per platform using the scale below. Leave the score out when the evidence for that stage is too thin, because a blank is more useful than a guess. The summary tab shows "stages scored" so readers can weigh averages.

| Score | Meaning |
|---|---|
| 5 Exemplary | Frictionless. Copy this pattern. |
| 4 Good | Works well, minor nitpicks. |
| 3 Acceptable | Gets the job done, nothing memorable. |
| 2 Poor | Noticeable friction, hesitation or backtracking. |
| 1 Broken | Would abandon here. |

### 7. Patterns and friction

- **Patterns tab**: 10–20 entries. Most should be `Copy` and `Avoid`; use `Reference` for useful non-competitor examples (e.g. a local wallet app). Each entry needs the specific Mobbin flow or screen URL.
- **Friction log**: 8–15 concrete moments, each with severity 1–3, a link, and a design implication for the user's own flow.

### 8. Build the workbook

1. Write the spec JSON (format in `references/spec-format.md`; full worked example in `assets/example-hotel-b2c-spec.json`). Include Read Me sections covering method, what Mobbin can't show, capture context per app, platform set and coverage gaps, how to use, and the scoring scale.
2. Run `python3 ${CLAUDE_PLUGIN_ROOT}/skills/mobbin-competitive-benchmark/scripts/build_benchmark.py spec.json ./<name>.xlsx` (needs `openpyxl`; `pip install openpyxl` if missing).
3. Formulas have no cached values until recalculated. If `soffice` is available, run `soffice --headless --convert-to xlsx --outdir ./recalc ./<name>.xlsx` and use the recalculated copy; otherwise tell the user the averages compute when the file is opened in Excel or Sheets. Spot-check the Scoring Summary averages either way.
4. Present the file.

Deliver an .xlsx by default, since the matrix and auto-scoring are spreadsheet-native. If the user asks for a doc or slides instead, reuse the same research and change only the output.

### 9. Summarise in chat

Keep it brief, because the workbook holds the detail. Cover:
- what's in the file,
- coverage gaps (apps not on Mobbin, stages not captured, market mismatch) and what to self-test first,
- whether criteria were rebuilt rather than copied from the user's kit,
- the 3–4 most important findings.

## Rules of thumb

- Cite every Mobbin claim through the Evidence Index or a pattern link. Stakeholders will ask "where did you see that?"
- Mobbin captures can be months or years old; say so, and use the dates visible in screenshots.
- Don't pad the matrix to look complete. A yellow cell that says "self-test" is more trustworthy than a plausible guess.
- Keep the user's market in view: a pattern that is best-in-class in the US may be irrelevant (or wrong) for Indonesia.
