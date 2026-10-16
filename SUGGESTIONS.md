# Suggested additions (not implemented)

Each item: what · why · who · size (S/M/L) · needs backend?

## Real MVP gaps

| What | Why | Who | Size | Backend |
|------|-----|-----|------|---------|
| Auth (email magic link or OAuth) with org membership | Prototype has no identity; partners cannot safely see portfolios | All | L | Yes |
| Multi-tenant orgs: Startup / Incubator / Law firm | Amity-like centres and firms need isolated tenants | Incubator, Law | L | Yes |
| Sharing “grant” model: startup owns data; partners get scoped, revocable, logged access | Matches the landing copy promise; required for trust | Startup, Incubator, Law | L | Yes |
| Append-only audit log (who saw/edited/approved what) | Law firms and incubators need evidence of process | Law, Incubator | M | Yes |
| Roles & permissions (Founder, Mentor, Associate, Partner Approver, Auditor RO) | Partner Approver gate is demo-only today | Law, Incubator | M | Yes |
| Evidence upload (files + links) with retention labels | “Attach evidence” is a toast stub | Startup, Law | M | Yes |
| Persistence of assessments, plans, drafts, comments | Everything is in-memory sample state | All | L | Yes |
| Real review requests that appear in counsel queue | Startup “Request review” does not create a Law matter | Startup, Law | M | Yes |
| Notifications & reminders (email; later Slack) | Incubator nudge is demo-only | Incubator, Startup | M | Yes |
| CSV bulk invite (parse + validate + progress) | UI pretends; no parsing | Incubator | S | Yes |
| PDF/CSV export of cohort report and audit report | Preview only | Incubator, Law | M | Yes |
| Content-pack versioning with “guidance as of” dates | AS_OF is hard-coded; packs must evolve with rules | All | M | Yes |
| Hindi (+ later regional languages) for startup path | Grade 8–10 English only today | Startup | L | Partial (i18n files can start static) |

## Product / flow gaps

| What | Why | Who | Size | Backend |
|------|-----|-----|------|---------|
| Empty / error / offline states | Demo always has sample rows | All | S | No |
| Undo for mark-done / owner / comment | Easy to mis-click in checklist | Startup, Law | S | No |
| Programme setup wizard (deadline, pack, white-label) | Cohort 2026 is hard-coded | Incubator | M | Yes |
| Playbook library (editable question sets, clauses) | Firm playbook is fixed sample | Law | L | Yes |
| Billing placeholders (cohort licence / per-matter) | Needed for Amity + firm packaging conversations | Incubator, Law | S | Later |
| Onboarding tour for first-time founders | First viewport is dense for non-lawyers | Startup | S | No |
| “Consult a lawyer” handoff with pre-filled brief | High-risk SDF path should export a counsel pack | Startup, Law | M | Yes |

## Security & privacy (product must be DPDP-aware)

| What | Why | Who | Size | Backend |
|------|-----|-----|------|---------|
| Data minimisation: store answers needed for plan, not free-text PII dumps | Tool itself processes personal data | All | M | Yes |
| Encryption at rest + TLS; secrets management | Multi-tenant evidence vault | All | M | Yes |
| Retention & deletion controls for org data and exports | Users must be able to leave / erase | Startup | M | Yes |
| Explicit consent for sharing with incubator/firm (already sketched in UI) | Make grants enforceable and revocable | Startup | M | Yes |
| CSP, secure headers, no unexpected third parties beyond fonts | Prototype loads Google Fonts only — keep that discipline | All | S | Ops |
| Dependency / supply-chain policy when leaving single-file HTML | Prevent drive-by npm sprawl | Eng | S | Process |

## Accessibility (WCAG AA — short audit)

Failures / gaps observed or likely after fixes:

1. **Focus trap / return** — addressed for drawer/modal; re-test with screen readers.
2. **Tablist** — arrows + `tabpanel` added; still missing `aria-orientation` and visible focus on all chips/filters in places.
3. **Tables** — rows now keyboard-activatable; checkbox + row activation may confuse AT (prefer a dedicated “Open” control).
4. **Colour alone** — readiness Red/Amber/Green uses colour + shape (tri/sq); keep text labels (present) and verify contrast of chips on dark.
5. **Name, Role, Value** — custom `opt` buttons use `aria-pressed` (good); progress bar on assess is `aria-hidden` (should expose value).
6. **Motion** — none today; if adding toasts animations, respect `prefers-reduced-motion`.
7. **Zoom 200%** — not fully re-verified after shell change; re-check sticky bar wrapping and workbench stack.
8. **Language** — `lang="en"` set; Hindi UI will need `lang` switching per view.

## Testing & architecture path

**Keep:** Single compliance engine (`QS` / `qList` / `pruneAns` / `derive` / `genDoc`), three surface shells, brand tokens, disclaimer pattern, human sign-off (never auto-certify).

**Proposed path off the prototype:**

1. Extract engine to `engine/` (pure TS, no DOM) with unit tests for every branching path and prune cases.
2. Thin React (or similar) apps: `apps/startup`, `apps/partner`, shared `packages/ui`.
3. API: Postgres + row-level org scoping; object storage for evidence; queue for invites/reminders.
4. Data model sketch: `Org`, `Membership`, `Programme`, `StartupProfile`, `Assessment`, `Answer`, `PlanItem`, `Document`, `Grant`, `Matter`, `Comment`, `Approval`, `AuditEvent`, `ContentPack`.
5. Keep Playwright journeys; add engine unit tests as the source of truth for DPDP heuristics (lawyer-reviewed fixtures).

Rough sequencing for an Amity-first launch: auth + startup persistence → incubator programme + grants → counsel review queue → firm playbooks/billing.
