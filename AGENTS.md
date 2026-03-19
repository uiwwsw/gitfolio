# AGENTS.md

Repository-wide guidance for agents working in `gitfolio`.

## Product boundaries

- Use only public GitHub evidence. Do not assert tenure, leadership, collaboration quality, or business impact unless the public evidence explicitly supports it.
- This product targets individual developer accounts. Do not expand organization-account behavior unless the task explicitly requires that product change.
- Treat benchmark output as an interpretive aid, not a hard truth claim.

## Preferred change surface

- Prefer data changes before engine changes.
  - Update `data/signals/*.json`, `data/rules/*.json`, or `data/repo-identity/rules.json` first when adjusting inference behavior.
  - Only change `lib/profile-features.ts`, `lib/rule-engine.ts`, or `lib/repo-identity.ts` when the data model itself is insufficient.
- Keep result-page changes aligned across both locales.
  - Check `app/result/page.tsx` and `app/en/result/page.tsx`.
  - Check dictionary coverage in `lib/i18n.ts` for any new user-facing text.

## Benchmark rules

- Do not render raw benchmark percentiles directly as `Top N%` or `상위 N%`.
  - Use helpers in `lib/benchmark-presentation.ts` for display labels and conservative band fallback.
- If you change the benchmark snapshot shape or `buildBenchmarkSnapshot`, update all dependent paths together:
  - `lib/analyze.ts`
  - `components/result/common.tsx`
  - `components/templates/brief.tsx`
  - `components/templates/insight.tsx`
  - `scripts/run-quality-regressions.cjs`
- Keep low-confidence or small-sample benchmark output conservative. Exact percentages should not bypass the presentation helper.

## Result document rules

- The result page is a print-first document. Preserve A4/browser-PDF behavior when editing:
  - `components/result/result-actions.tsx`
  - `components/result/document-shell.tsx`
  - `app/globals.css`
- Download filename behavior depends on the result-document helpers and print title override.
  - If you change export naming, update both `lib/result-document.ts` and `components/result/result-actions.tsx`.
- SEO/title/favicon changes should go through `lib/seo.ts` and the App Router metadata flow.

## Development workflow

- Prefer fixture mode for UI work when live GitHub data is unnecessary:
  - `GITFOLIO_USE_FIXTURE=1 npm run dev`
- Avoid committing generated-file churn.
  - Revert accidental edits to `next-env.d.ts` before finishing.

## Validation

- For TypeScript or UI changes: run `npm run typecheck`.
- For scoring, benchmark, template, or inference changes: run `npm run quality:regress`.
- Before shipping broader changes: run `npm run check`.

