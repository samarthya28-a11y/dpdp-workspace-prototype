# Wording to flag (do not edit in code)

Lawyer review pending. Line numbers refer to `dpdp-prototype.html` as of the audit/fix branch. Left unchanged on purpose.

| Line(s) | Text (summary) | Why flag |
|---------|----------------|----------|
| 219 | Volume why: “Bigger databases can mean **stricter rules**.” Options include **50,000 to 10 lakh** / **More than 10 lakh**. | Implies volume thresholds create legal duties. DPDP Significant Data Fiduciary criteria (when notified) may not match these heuristic bands; numbers may mislead founders. |
| ~248–250 (`derive` sdf) | SDF heuristic: volume `big`, or `mid`+sensitive, or `auto` yes/ns. | Uses internal “SDF” logic without saying “Significant Data Fiduciary” on the result, but counsel callout talks about “stricter duties”. Risk of users equating the flag with a legal SDF determination. |
| 264 | “**Consent must be** free, specific and easy to withdraw. No pre-ticked boxes.” | Presents consent standards as absolute for all purposes; DPDP also contemplates certain legitimate uses. May over-state that every processing needs a tick-box. |
| 266 | Breach: “If data leaks you **must** act fast and **tell the right people**.” | “Must” + unspecified recipients; notification duties/timelines depend on notified rules and facts. |
| 267 | Children’s data: parent/guardian consent; “**no tracking or targeted ads**.” | Strong absolute; confirm against final rules/guidance for under-18s. |
| 272–273 | Processor: “The business that hires you carries the **main duties**. The law **expects a contract**…” | “Expects a contract” may be stronger/weaker than the Act’s actual processor provisions; needs counsel pin-cite. |
| 333 | “In DPDP terms that **usually makes you** a Data Processor / Data Fiduciary.” | Even with “plain-language guess”, “usually makes you” is easy to read as a classification. |
| 338 | “Only a lawyer can say whether **stricter duties** apply to you.” | Tied to heuristic risk; users may think “Higher” = SDF. |
| 218 | Examples list **Aadhaar**, PAN, passport as ID documents. | Sensitive operationally and politically; confirm whether product should name Aadhaar in examples. |
| 371 | ASSUME volume: “You hold details of **50,000 to 10 lakh** people” when user said Not sure. | “Careful answer” assumption may not be the most conservative for every obligation; counsel should confirm assumption map. |
| 395 | Privacy notice: complain to the **Data Protection Board of India**. | Confirm Board name, standing, and when this sentence is appropriate for every fiduciary draft. |
| 390–392 | Consent copy: marketing tick-box; order flow “[[Ask your lawyer whether order data needs a tick-box or only this notice]]”. | Good caution, but surrounding copy still models one pattern; easy to ship unchanged. |
| 212–213 | Minors / tracking questions. | Definitions of “under 18”, “track”, and “ads” are product language, not statutory. |
| 215 | “Data stored or processed **outside India**.” | Cross-border transfer regime depends on government notifications; “outside India” alone may over-simplify. |
| 216 | Automated decisions examples: credit, screening, prices. | Maps to “software alone” heuristic for SDF-like flag; may not match any notified criterion. |
| 567 | Law workbench: “Possible **SDF indicator**: not triggered (volume below 10 lakh…)”. | Shows associates a numeric SDF rule that is product-invented; dangerous if pasted into advice. |
| 605 | Partner sign-off: “**professional advice** for this client only…” | Template may create professional-responsibility implications for firms using the demo text. |
| Incubator strip / Green rate | “Green” readiness | Copy elsewhere says colour ≠ compliance (good); still watch any future badge language. |

## Thresholds / claims left untouched

- Volume bands and SDF heuristic in `derive`
- Consent / breach / children’s / cross-border plan item wording
- Role labels Data Fiduciary / Data Processor on the result screen
- Document drafts (notice, consent, DPA checklist, staff notice)
- Global disclaimer and “guidance as of” date (`AS_OF`)
- Partner approval statement in the lock modal

If counsel rewrites any of the above, prefer a versioned content pack rather than one-off string edits.
