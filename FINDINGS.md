# DPDP prototype audit findings

Audit date: 16 Oct 2026. Method: full source read, isolated `derive`/`qList` logic checks, Playwright exercises at 390 / 1280 px (light + dark). No production claims; sample data only.

| # | Severity | Area | How to reproduce | Expected | Actual | Proposed fix |
|---|----------|------|------------------|----------|--------|--------------|
| 1 | Blocker | Intake / `derive` | Answer **Yes** on “software alone” while it is shown (e.g. after ticking ID docs). Use **Change answers**, remove sensitive details so volume stays small and the auto question is hidden. Open result. | Hidden answers must not affect SDF / counsel callout / plan. | `auto: 'yes'` remains in `S.st.ans`; `qList` hides auto; `derive` still sets `sdf=true` and shows counsel callout. | Add `pruneAns(a)` that keeps only keys present in `qList(a)` (iterate until stable). Use pruned answers in `derive`, summary, plan, docs. |
| 2 | Major | A11y / drawer & modal | Open a plan item drawer (or any modal). Press **Tab** repeatedly. | Focus stays inside the dialog. Focus returns to the opener on close. | Focus escapes to the page behind. No stored return focus. | Focus trap on `.panel` / `.modal .box`; save `document.activeElement` on open; restore on close / Escape. |
| 3 | Major | A11y / tables | Incubator or Law queue: use keyboard only. | Rows that open a drill-down / matter are reachable and activatable. | `tr[data-a]` has no `tabindex`; click-only. | `tabindex="0"` on actionable rows; Enter/Space triggers the same action as click. |
| 4 | Major | Brand / modals | Open **Edit name**, then **Bulk invite**. Change Sample incubator / product name while the modal is open. | Invite preview and logo reflect live brand settings. | Logo updates; modal body keeps the old partner / product strings. | Build modals from a factory function and re-run it on brand `input` when a modal is open. |
| 5 | Major | Law workbench | Add a comment → **Request changes** → **Simulate client resubmit**. | After resubmit, comments are resolved; **Request changes** disabled until new comments exist. Version bumps. | Version bumps and stage returns to review, but `comments.length` still > 0 so **Request changes** stays enabled. | Gate on unresolved comments (`item !== 'resolved'`); use that list in the changes modal draft. |
| 6 | Major | Law workbench | Type in **Internal notes**, then **Recommend for approval** (or any action that `render()`s). | Note text persists (`S.law.note`). | `#inote` is empty after re-render; `S.law.note` is never written. | Bind `#inote` like document fields (`input` → `S.law.note`); set `value` from state in `vLaw`. |
| 7 | Minor | A11y / tabs | Focus Startup tab, press ArrowRight / ArrowLeft. | Moves selection across the tablist; panels labelled. | Arrow keys do nothing; no `aria-controls` / `tabpanel`. | WAI-ARIA tabs: arrows, Home/End; `role="tabpanel"` on `#view`. |
| 8 | Minor | Documents | In Business name, type `[[secret]]`. | User text is literal (gaps only from empty template fields). | `ph()` turns user `[[…]]` into `<mark>`. | Use a private sentinel for empty-field gaps instead of `[[…]]` in `F()` / `ph()` / gap counter. |
| 9 | Minor | HTML | Open source. | Valid document shell. | File starts at `<title>` (no doctype / html / head / body). | Wrap with `<!DOCTYPE html><html lang="en">…` (no copy changes). |
| 10 | Minor | A11y | Incubator table first column header. | SR-only “Select” label. | `class="sr" hidden` — `hidden` removes it from the a11y tree; no `.sr` CSS. | Clip-based `.sr` styles; drop `hidden`. |
| 11 | Minor | Brand | Open invite modal, then try **Edit name**. | Documented behaviour. | Modal overlay blocks the button (brand panel still editable if opened first). | No code change required; noted for UX. Prefer opening brand panel before modals in demos. |

## Suspected issues — confirmation

| Suspect | Result |
|---------|--------|
| Stale answers feeding `derive` | **Confirmed (Finding 1).** Clearest case: hidden `auto`. Processor path with stale `minors` does *not* add children’s plan items (`fid` false). `collect: 'no'` correctly clears fiduciary flags even with leftover answers. |
| Click handling / `data-stop` | **OK in audit.** Clicks on drawer panel content do not close; backdrop closes. |
| Focus trap / keyboard rows / tablist | **Confirmed (Findings 2, 3, 7).** Escape closes drawer. |
| Re-render loses field values | Doc fields: **OK** (targeted `#docPaper` update). Mentor note: OK after Add. Internal note: **broken (Finding 6)**. Brand + open modal: **stale modal HTML (Finding 4)**. |
| Layout @390px | Tables scroll inside `.tbl`; no finding for page-level overflow in this run. |
| Dark mode contrast | `<mark>` gold / on-gold measured ≥ 4.5:1 in dark. Locked / chip colours acceptable in spot check. |
| Law stages / version / role gate | Resubmit → review + v3 **OK**. Associate cannot approve **OK**. Request-changes enablement after resolve **broken (Finding 5)**. |
| Document studio | XSS via `<b>` in fields: escaped. Active-tab fallback when role changes: OK. User `[[` gaps: Finding 8. |
| Intake “Not sure” / processor docs | NS raises SDF + assumed list **OK**. Processor-only skips notice/consent docs and shows client contract / sub-vendor **OK**. |

## Console

No page errors or console errors on the main Startup / Incubator / Law paths exercised.
