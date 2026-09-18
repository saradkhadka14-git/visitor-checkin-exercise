# Visitor Check-in — QA Notes

**Status: COMPLETE for the recorded QA execution; 62 cases catalogued (52 pass, 8 fail, 2 N/A).**

## Highest-risk area

The highest-risk area is the visitor lifecycle and roster accuracy: registration,
checkout, deactivation, repeat visits and pagination all determine who a
receptionist believes is currently on site. The execution found two related
checkout-state problems (the API returns `active:true` after checkout and the
UI can disable Next while another page still contains visitors) and two
deactivation problems (a deactivated visitor remains in the active list and in
repeat-visit suggestions). These can produce an incorrect operational roster
or permit a visit that administration has explicitly disabled.

Time display is the next-highest risk. The API stores UTC correctly, but the UI
renders the UTC hour/minute instead of the confirmed browser timezone
`Asia/Katmandu`; visitor 89 displayed `05:32` where Nepal local time is `11:17`.
This is a user-visible error even when registration and persistence are
otherwise correct.

## Execution summary

| Result | Count | Notes |
|---|---:|---|
| `[pass]` | 52 | Core flow, host mapping, validation, API/pagination boundaries, text handling, zoom, repeat visit, and successful POST evidence. |
| `[fail]` | 8 | Whitespace-only name, whitespace normalization, checkout state, checkout pagination, timezone display, and deactivation list/search exclusion. |
| N/A | 2 | Search-only UI case and Back/Forward case are not meaningful features in the supplied app. |
| `[ ]` / partial | 0 | No executable case remains unrecorded in the final suite. |

The failure count represents test cases, not unique defects: checkout state is
intentionally covered by two API cases, while deactivation has separate list
and repeat-search cases. The original Word execution report had 59 cases; this
suite adds the three follow-up cases QA-VIS-060–062 and updates QA-VIS-001,
QA-VIS-003 and QA-VIS-058 with the later evidence.

## One specific Product Owner question before sign-off

**Which of full name, company name, host employee and purpose must be
mandatory through both the UI and API, and should a whitespace-only value count
as missing?**

The UI marks Full Name and Host as required, while the API model allows an
optional host and has no equivalent validation contract. The execution found
that browser-native required validation works, but whitespace-only Full Name is
accepted and persisted. This question is required before deciding whether the
API should reject omitted/blank optional fields and before closing the
whitespace defects.

## Regression subset for a minor registration-form update

Assumption: the change only adjusts registration layout, controlled inputs or
validation and does not intentionally change the API contract, list query,
checkout, deactivation or database. If the diff touches host/search APIs or a
shared list component, run the full suite.

**Include:** QA-VIS-001, 002, 003, 004, 005, 007, 008, 009, 010, 011, 012,
014, 015, 016, 018, 019, 020, 021, 022, 024, 025, 026, 028, 029, 030, 031,
034, 037, 041, 050, 058, 062.

These cover payload mapping, host initialization, required/whitespace rules,
autocomplete and stale suggestions, error/retry behavior, form reset, text
boundaries, the timestamp created by registration, list refresh integration,
zoom usability and a checked-out repeat visit.

**Exclude from the minor-form subset:** QA-VIS-006, 013, 017, 023, 027, 032,
033, 035, 036, 038, 039, 040, 042–049, 051–057, 059–061.

These are either checkout/deactivation/server-state tests, broad pagination or
API parameter boundaries, performance, unsupported browser history, or known
defects outside a form-only change. They still belong in full-release
regression; exclusion is not a pass or waiver. QA-VIS-013 and QA-VIS-056 are
N/A in the current product and remain documented for traceability.

## Limitations and open questions

- No application code was changed because this is the QA Track; defects are
  documented for the development track.
- The terminal `GET /api/visitors?page=4` response was `[]` after Page 3. The
  specification does not define terminal-page messaging, so it is recorded as
  an open UX question rather than a defect.
- SQL query counts and latency measurements were not required to close the QA
  suite. The source-level host lookup/N+1 concern is retained in
  `defect-report.md` as a performance risk, not a measured SLA failure.
- Controlled network/race cases are explicitly labelled as simulated where
  the browser could not reproduce them safely; they are not presented as live
  production observations.

## Submission checklist

- Keep `defect-report.md`, `qa-suite.md`, `qa-notes.md` in the repository root.
- Include the dated evidence under `qa-evidence/`.
- Create `submission/<your-name>` from the actual repository main branch,
  commit the reviewed files, push without force, and open a PR against `main`.
- Put the PR URL in the submission email before the stated 2026-09-19 deadline.

