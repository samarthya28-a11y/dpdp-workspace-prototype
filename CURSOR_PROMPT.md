# Cursor task: review, fix and extend the DPDP workspace prototype

## What this is
`dpdp-prototype.html` is a single-file, no-build clickable prototype of a DPDP (India's Digital Personal Data Protection Act) compliance platform. It has three surfaces on tabs:

1. **Startup**: landing, adaptive assessment, result, roadmap, documents studio.
2. **Incubator**: cohort portfolio, mentor drill-down, bulk actions, cohort report preview.
3. **Law firm**: review queue and a three-pane matter workbench with comments, partner approval, version lock and report outline.

The product name is a live setting ("Edit name" button) and must stay swappable. The name "Setu by AWI" is provisional and under trademark review.

It is a **prototype, not a finished product**: no backend, no auth, no persistence, all data is sample data. Do not treat it as production code.

## Ground rules
- Keep it a single HTML file with vanilla JS unless I say otherwise. No framework, no build step, no new dependencies.
- Allowed external loads: Google Fonts stylesheet only. No other network calls.
- **Do not change legal wording**, thresholds or compliance claims (intake questions, roadmap text, document drafts, disclaimers, sign-off statement). Those are waiting on lawyer review. If you think a wording is wrong or risky, list it under "Wording to flag" and leave it as is.
- Never add a "compliant", "certified" or similar badge or claim. Approval is always a named human step.
- The disclaimer must stay visible wherever obligations or documents appear.
- Keep light and dark themes both working (CSS custom properties on `:root`, dark overrides under `prefers-color-scheme` and `[data-theme]`).
- Keep the product name and "by" company and sample incubator name editable and used everywhere. Grep for hard-coded "Setu" and "AWI" outside the defaults and fix any.

## Step 1: audit first, change nothing yet
Read the whole file, then run it in a browser (Playwright is fine) and exercise every path. Produce a findings table before editing: `#`, severity (Blocker / Major / Minor), area, how to reproduce, expected vs actual, proposed fix.

Paths to cover:
- Startup: landing, then every question, back and forward, change earlier answers via "Change answers", result, plan, open each item, mark done, owner change, documents (all tabs), edit every field, request review, copy as text.
- Answer sets to try: processor only; own customers only; both; "Not sure" on everything; "No" to collecting data; sensitive details with large volume.
- Incubator: every filter, select rows, send reminder, assign mentor and counsel, open and close drill-down, add note, bulk invite, export preview.
- Law firm: queue, open matter, switch role Associate and Partner, comment on a paragraph, request changes, simulate resubmit, recommend, approve and lock, report modal.
- Brand panel: change all three names while a modal or drawer is open and while on each tab.
- Viewports: 390px, 768px, 1280px. Light and dark. Keyboard only. Zoom 200%.

## Suspected issues to confirm first
1. **Stale answers.** Answers to questions that later become hidden (for example after changing the "whose customers" or "do you collect" answer on "Change answers") may still feed `derive()` and change the result, SDF flag or plan. Hidden answers should be ignored or cleared.
2. **Click handling.** The global click handler uses a convoluted `data-stop` check for drawer and modal overlays. Confirm clicks inside panels never close them and that overlay clicks do.
3. **Focus and keyboard.** No focus trap or focus return for the drawer and modals. Table rows with `data-a` are mouse-only. `role="tab"` buttons have no arrow-key handling or tabpanel. Hidden header cell uses an odd `.sr` pattern.
4. **Re-render side effects.** `render()` rebuilds `#view` and `#layer`. Check that typing in document fields, the mentor note, the comment box and the internal note keeps focus and value, and that editing the brand name while a modal is open does not break anything.
5. **Layout.** Tables use `min-width`; check phone width for horizontal page scroll. Check the three-pane workbench and documents grid when stacked.
6. **Dark mode contrast.** `mark`, chips, ribbons, the `.locked` block and status colours.
7. **Law workbench state.** Comments marked `resolved` after resubmit; role switch at each stage; whether "Request changes" is correctly disabled with no comments; version numbers.
8. **Document studio.** Empty fields, very long values, special characters and `[[` in input; the gap counter; the active-tab fallback when the role changes.
9. **Intake logic.** Confirm "Not sure" on details, volume and software decisions raises the flags as the result screen says; confirm the processor-only path skips the right questions and shows no consent notice.

## Step 2: fix
Fix Blockers and Majors, then Minors where cheap. Small, separate commits, each naming the finding number. Add a lightweight Playwright smoke test (`tests/smoke.spec.js` or similar) that walks the main path on each tab and fails on any console error or uncaught exception, and a test for each fixed bug.

## Step 3: suggest additions (do NOT implement)
Write `SUGGESTIONS.md`. For each suggestion give: what, why, which user it helps (startup, incubator, law firm), rough size (S/M/L), and whether it needs a backend. Cover at least:
- What is missing for a real MVP: auth, multi-tenant organisations, the sharing "grant" model (startup owns data, partners get scoped, revocable, logged access), append-only audit log, roles and permissions, evidence upload, persistence, real review requests that appear in the counsel's queue, notifications and reminders, CSV bulk invite, PDF/CSV export, content-pack versioning with "guidance as of" dates, Hindi and other Indian languages.
- Product gaps in the flows: onboarding, empty and error states, undo, programme setup and deadlines for incubators, playbook library for law firms, reports, billing placeholders.
- Security and privacy of the product itself (it must be DPDP-aware): data minimisation, encryption, retention and deletion controls, consent for sharing with incubators, CSP and headers, dependency policy.
- Accessibility to WCAG AA: a short audit with specific failures.
- Testing and architecture: a proposed path from this prototype to a real app (stack, data model sketch, what to keep).

## Step 4: wording to flag
Add a `WORDING_FLAGS.md` listing any text you think is legally risky, unclear, or misleading to a non-lawyer, with the line and why. Do not edit it.

## Deliverables
1. Findings table (in the PR description or `FINDINGS.md`).
2. Fix commits and the smoke test.
3. `SUGGESTIONS.md` and `WORDING_FLAGS.md`.
4. A short summary: what was fixed, what was not, and what you need from me.

## Ask before you
- Add a dependency, a build step or any network call.
- Change any legal wording, threshold or disclaimer.
- Delete a feature.
