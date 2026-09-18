# Visitor Check-in — QA Suite

**Status: EXECUTED — 62 cases: 52 [pass], 8 [fail], 2 N/A.**

Markers: [pass] means the observed result matched the expected result; [fail] means it contradicted the expected result; N/A means the current product does not expose a meaningful target for that case. Every test title uses “Verify”. Every step begins with “Check” or “Confirm”. Latest evidence is in qa-evidence/user-execution-2026-09-10.md, qa-evidence/user-execution-2026-09-16.md and qa-evidence/user-execution-2026-09-17.md.

## Test environment and execution notes

- API: http://localhost:3000/api; UI: http://localhost:5173; SQLite; browser timezone Asia/Katmandu (Nepal).
- User-run environment: Rails 7.2.3.2/Puma 8.0.2, Ruby 3.3.12, Vite 8.2.2, Node 24.19.0, Chromium-based browser.
- API routes exercised: GET /hosts, GET /visitors?page=N, POST /visitors, GET /visitors/search?q=..., PATCH /visitors/:id/check_out, PATCH /visitors/:id/deactivate.
- Seed data was not changed. Dedicated names such as QA Deactivate 060, QA Repeat 062 and QA POST Response 001 were used.
- The empty GET /api/visitors?page=4 terminal page was recorded as an open UX question, not a defect: the specification requires 20-record pagination but does not specify terminal-page messaging.
- API UTC timestamps are valid storage/transport values; QA-VIS-030/D01 separately checks local rendering.

## Test cases

## QA-VIS-001 — Verify registration with all feature fields

- **Preconditions:** Healthy API/UI and a valid host.
- **Steps:** 1. Check the form with QA POST Response 001, QA Company, Alice Mercer and POST response capture; confirm the Network POST and refreshed list.
- **Expected result:** One visitor is created with HTTP 201, correct fields, checked_in_at, active true and checked_out_at null.
- **Result:** [pass]
- **Actual result / evidence:** User Network/API evidence 2026-09-17; response id 123.
- **Related defect:** —

## QA-VIS-002 — Verify selected host is saved and displayed correctly

- **Preconditions:** At least two hosts are available.
- **Steps:** 1. Check a registration using Benjamin Okafor; confirm host_id and displayed host_name.
- **Expected result:** The selected employee ID and name match in the API and list.
- **Result:** [pass]
- **Actual result / evidence:** QA Host 002 / id 84 / host_id 2.
- **Related defect:** —

## QA-VIS-003 — Verify submitted text is preserved

- **Preconditions:** Valid host; company North & South; purpose Budget review — phase 2.
- **Steps:** 1. Check registration with the exact punctuation and spacing; confirm the response and refreshed row.
- **Expected result:** Name, company and purpose are stored and displayed without swapping or corruption.
- **Result:** [pass]
- **Actual result / evidence:** Retest rows QA Text Preservation 003 and Retest.
- **Related defect:** —

## QA-VIS-004 — Verify host options are usable on initial load

- **Preconditions:** Healthy GET /api/hosts.
- **Steps:** 1. Check the form after hosts load and select Alice Mercer; confirm a registration succeeds.
- **Expected result:** Known host options are selectable and the selected host is submitted correctly.
- **Result:** [pass]
- **Actual result / evidence:** QA Host Load 004 / id 89.
- **Related defect:** —

## QA-VIS-005 — Verify checkout returns a consistent state

- **Preconditions:** An active visitor is visible and its checkout endpoint is available.
- **Steps:** 1. Check Check Out; confirm checked_out_at and active in the HTTP response.
- **Expected result:** Successful checkout sets checked_out_at and returns a state consistent with an inactive/checked-out visitor.
- **Result:** [fail]
- **Actual result / evidence:** IDs 115 and 117 returned active:true after checkout.
- **Related defect:** DEF-004

## QA-VIS-006 — Verify visitor-list pagination boundaries

- **Preconditions:** More than one populated page exists.
- **Steps:** 1. Check Previous and Next from first, middle and last populated pages; confirm the page rows change correctly.
- **Expected result:** Populated pages are reachable and Previous/Next boundary controls behave safely.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-006.
- **Related defect:** —

## QA-VIS-007 — Verify required fields block an empty submission

- **Preconditions:** The form is loaded with Full Name and Host marked required.
- **Steps:** 1. Check Submit with every field empty; confirm browser validation and no new row.
- **Expected result:** The UI blocks the submission with required-field feedback.
- **Result:** [pass]
- **Actual result / evidence:** Browser native validation observed.
- **Related defect:** —

## QA-VIS-008 — Verify whitespace-only Full Name is rejected

- **Preconditions:** Valid company, purpose and host; Full Name contains only spaces.
- **Steps:** 1. Check Submit; confirm whether a visitor row is created.
- **Expected result:** A meaningful-name policy should reject a trimmed-empty name; no blank-name row should be created.
- **Result:** [fail]
- **Actual result / evidence:** Blank-name row was created.
- **Related defect:** DEF-002

## QA-VIS-009 — Verify missing host is blocked in the UI

- **Preconditions:** Full Name is filled; no host is selected.
- **Steps:** 1. Check Submit; confirm the host required message and no new row.
- **Expected result:** The UI blocks submission until a host is selected.
- **Result:** [pass]
- **Actual result / evidence:** Browser native host validation observed.
- **Related defect:** —

## QA-VIS-010 — Verify a successful new visitor check-in

- **Preconditions:** Unique valid data and host.
- **Steps:** 1. Check Submit; confirm the new row and API record.
- **Expected result:** The new visitor appears with supplied values, active true and a check-in timestamp.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-010.
- **Related defect:** —

## QA-VIS-011 — Verify the form resets after successful check-in

- **Preconditions:** A valid visitor can be submitted.
- **Steps:** 1. Check a successful submission; confirm all registration controls clear and a second empty submit is blocked.
- **Expected result:** Successful submission clears the form without creating a second record.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-011.
- **Related defect:** —

## QA-VIS-012 — Verify duplicate submit protection

- **Preconditions:** A valid form and a delayed or rapid-submit condition.
- **Steps:** 1. Check two rapid Submit activations; confirm request/record count.
- **Expected result:** The agreed duplicate policy is respected; execution produced one record.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-012.
- **Related defect:** —

## QA-VIS-013 — Verify visitor search UI availability

- **Preconditions:** Open the current frontend.
- **Steps:** 1. Check for a dedicated historical search/filter control; confirm whether the feature exists.
- **Expected result:** If no search UI is specified, record N/A rather than treating absence as a defect.
- **Result:** N/A
- **Actual result / evidence:** Current app has no separate search UI; repeat suggestions are covered elsewhere.
- **Related defect:** —

## QA-VIS-014 — Verify persistence after page refresh

- **Preconditions:** A newly created visitor is active.
- **Steps:** 1. Check the row, refresh the page and confirm the same record remains.
- **Expected result:** The persisted visitor remains in the active list after refresh.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-014.
- **Related defect:** —

## QA-VIS-015 — Verify host dropdown data loading

- **Preconditions:** Healthy hosts endpoint.
- **Steps:** 1. Check the dropdown options; confirm the expected unique host names and that they can be selected.
- **Expected result:** All returned hosts are displayed once and are usable.
- **Result:** [pass]
- **Actual result / evidence:** 12 unique hosts loaded.
- **Related defect:** —

## QA-VIS-016 — Verify long input handling

- **Preconditions:** Valid host and long name/company/purpose strings.
- **Steps:** 1. Check registration with long values; confirm the API and table layout.
- **Expected result:** The record saves and text remains usable without overlapping controls.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-016.
- **Related defect:** —

## QA-VIS-017 — Verify optional Company handling

- **Preconditions:** Valid name and host; company blank.
- **Steps:** 1. Check submission; confirm record creation and blank company representation.
- **Expected result:** Company remains optional under the observed contract and the visitor is created.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-017.
- **Related defect:** —

## QA-VIS-018 — Verify special-character handling

- **Preconditions:** Valid host.
- **Steps:** 1. Check apostrophe, hyphen, ampersand, slash and hash in text fields; confirm response and list.
- **Expected result:** Supported punctuation is preserved and safely rendered.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-018.
- **Related defect:** —

## QA-VIS-019 — Verify optional Purpose handling

- **Preconditions:** Valid name and host; purpose blank.
- **Steps:** 1. Check submission; confirm record creation and blank purpose representation.
- **Expected result:** Purpose remains optional under the observed contract and the visitor is created.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-019.
- **Related defect:** —

## QA-VIS-020 — Verify refresh during form entry

- **Preconditions:** Form contains unsaved values.
- **Steps:** 1. Check a browser refresh before Submit; confirm no partial visitor was created.
- **Expected result:** Unsaved form data may clear, but no partial server record is created.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-020.
- **Related defect:** —

## QA-VIS-021 — Verify repeated checkout is prevented in the UI

- **Preconditions:** An active visitor is visible.
- **Steps:** 1. Check its Check Out button once, then confirm the row is gone and cannot be checked out again from that page.
- **Expected result:** The UI does not offer a second checkout for the removed row.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-021.
- **Related defect:** —

## QA-VIS-022 — Verify checked-out visitor stays out after refresh

- **Preconditions:** A visitor has a successful checkout.
- **Steps:** 1. Check the active list and refresh; confirm the checked-out row does not return.
- **Expected result:** Checked-out visitors are excluded from active pages.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-022.
- **Related defect:** —

## QA-VIS-023 — Verify pagination remains usable after last-page checkout

- **Preconditions:** A visitor exists on the last populated page.
- **Steps:** 1. Check checkout of that last-page row; confirm the remaining controls and page state.
- **Expected result:** The list remains navigable after the mutation.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-023.
- **Related defect:** —

## QA-VIS-024 — Verify leading and trailing spaces are normalized

- **Preconditions:** Valid host; values include leading/trailing spaces.
- **Steps:** 1. Check registration; confirm exact stored API strings and visible text.
- **Expected result:** The approved normalization rule should be applied consistently before persistence.
- **Result:** [fail]
- **Actual result / evidence:** API id 107 retained a leading space.
- **Related defect:** DEF-003

## QA-VIS-025 — Verify very long Purpose wrapping

- **Preconditions:** A very long purpose string.
- **Steps:** 1. Check the row at normal zoom; confirm wrapping does not overlap adjacent cells.
- **Expected result:** Purpose wraps while table controls remain usable.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-025.
- **Related defect:** —

## QA-VIS-026 — Verify very long Company handling

- **Preconditions:** A very long company string.
- **Steps:** 1. Check the row at normal zoom; confirm wrapping and control alignment.
- **Expected result:** Company wraps without breaking the table.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-026.
- **Related defect:** —

## QA-VIS-027 — Verify very long Full Name handling

- **Preconditions:** A very long full name.
- **Steps:** 1. Check the row at normal zoom; confirm wrapping and action alignment.
- **Expected result:** Name wraps without hiding the host, time or action.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-027.
- **Related defect:** —

## QA-VIS-028 — Verify HTML input is rendered as text

- **Preconditions:** Full Name contains HTML tags.
- **Steps:** 1. Check registration and the list; confirm tags are displayed literally.
- **Expected result:** HTML is escaped and no markup is rendered.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-028.
- **Related defect:** —

## QA-VIS-029 — Verify script input is rendered safely

- **Preconditions:** Full Name contains a script tag.
- **Steps:** 1. Check registration; confirm no alert executes and the literal value is shown.
- **Expected result:** Script is treated as text and does not execute.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-029.
- **Related defect:** —

## QA-VIS-030 — Verify Unicode and Nepali text handling

- **Preconditions:** Valid host and Nepali name/company/purpose.
- **Steps:** 1. Check registration and refresh; confirm Unicode is preserved.
- **Expected result:** Nepali characters remain readable and uncorrupted.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-030.
- **Related defect:** —

## QA-VIS-031 — Verify keyboard navigation through registration

- **Preconditions:** Keyboard-only interaction is available.
- **Steps:** 1. Check Tab navigation through inputs, host and Submit; confirm the form submits without a mouse.
- **Expected result:** All controls are reachable in a sensible order and activation works.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-031.
- **Related defect:** —

## QA-VIS-032 — Verify keyboard checkout accessibility

- **Preconditions:** An active row and keyboard focus are available.
- **Steps:** 1. Check focus on Check Out and activate it from the keyboard; confirm the correct row is removed.
- **Expected result:** The action is keyboard reachable and performs the same checkout.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-032.
- **Related defect:** —

## QA-VIS-033 — Verify Full Name required validation

- **Preconditions:** Full Name is empty; other required controls are valid.
- **Steps:** 1. Check Submit; confirm the name validation and no new row.
- **Expected result:** The UI blocks submission.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-033.
- **Related defect:** —

## QA-VIS-034 — Verify Host required validation by keyboard

- **Preconditions:** Host is unselected; name is filled.
- **Steps:** 1. Check keyboard submission; confirm host validation and no new row.
- **Expected result:** The UI blocks submission until a host is chosen.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-034.
- **Related defect:** —

## QA-VIS-035 — Verify Company punctuation and numbers

- **Preconditions:** Company is Demo-123 and Co. Pvt. Ltd.
- **Steps:** 1. Check registration; confirm the exact company text.
- **Expected result:** The value is saved and displayed correctly.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-035.
- **Related defect:** —

## QA-VIS-036 — Verify checkout API state consistency on retest

- **Preconditions:** A newly created visitor is active.
- **Steps:** 1. Check checkout and inspect the API response; confirm both timestamps and active state.
- **Expected result:** checked_out_at is populated and active is false/consistent with checkout.
- **Result:** [fail]
- **Actual result / evidence:** QA Checkout Retest 036 returned active:true.
- **Related defect:** DEF-004

## QA-VIS-037 — Verify multiple visitors can share one host

- **Preconditions:** Valid host Alice Mercer and two unique visitors.
- **Steps:** 1. Check two registrations for the same host; confirm both rows map to Alice.
- **Expected result:** Both visitors are created independently with the same host.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-037.
- **Related defect:** —

## QA-VIS-038 — Verify one same-host checkout is independent

- **Preconditions:** Two active visitors share a host.
- **Steps:** 1. Check out only one; confirm the other row remains active with its host.
- **Expected result:** Only the selected visitor is removed.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-038.
- **Related defect:** —

## QA-VIS-039 — Verify different host assignment mapping

- **Preconditions:** Two unique visitors and two hosts.
- **Steps:** 1. Check one registration per host; confirm each row/API host mapping.
- **Expected result:** Each visitor displays the selected employee.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-039.
- **Related defect:** —

## QA-VIS-040 — Verify checkout across different hosts is independent

- **Preconditions:** Two active visitors use different hosts.
- **Steps:** 1. Check out one; confirm the other host/visitor remains unchanged.
- **Expected result:** Unrelated records are not mutated.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-040.
- **Related defect:** —

## QA-VIS-041 — Verify UI and API data consistency

- **Preconditions:** A unique visitor can be registered.
- **Steps:** 1. Check the row and corresponding API JSON; confirm name, company, purpose and host match.
- **Expected result:** Displayed and API values agree.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-041.
- **Related defect:** —

## QA-VIS-042 — Verify UI and API pagination consistency

- **Preconditions:** A populated Page 3 exists.
- **Steps:** 1. Check UI Page 3 and GET /api/visitors?page=3; confirm IDs match.
- **Expected result:** The same records appear in both representations.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-042.
- **Related defect:** —

## QA-VIS-043 — Verify out-of-range page handling

- **Preconditions:** API is running.
- **Steps:** 1. Check GET /api/visitors?page=999; confirm response and server stability.
- **Expected result:** An empty array is returned without a server error.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-043.
- **Related defect:** —

## QA-VIS-044 — Verify nonnumeric page handling

- **Preconditions:** API is running.
- **Steps:** 1. Check GET /api/visitors?page=abc; confirm a controlled response and server stability.
- **Expected result:** The server remains stable and applies its default/controlled behavior.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-044.
- **Related defect:** —

## QA-VIS-045 — Verify page-zero handling

- **Preconditions:** API is running.
- **Steps:** 1. Check GET /api/visitors?page=0; confirm no server error.
- **Expected result:** The server remains stable and returns a controlled/default page.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-045.
- **Related defect:** —

## QA-VIS-046 — Verify negative-page handling

- **Preconditions:** API is running.
- **Steps:** 1. Check GET /api/visitors?page=-1; confirm no server error.
- **Expected result:** The server remains stable and returns a controlled/default page.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-046.
- **Related defect:** —

## QA-VIS-047 — Verify empty-page-parameter handling

- **Preconditions:** API is running.
- **Steps:** 1. Check GET /api/visitors?page=; confirm no server error.
- **Expected result:** The server remains stable and returns a controlled/default page.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-047.
- **Related defect:** —

## QA-VIS-048 — Verify decimal-page handling

- **Preconditions:** API is running.
- **Steps:** 1. Check GET /api/visitors?page=1.5; confirm no crash and controlled response.
- **Expected result:** The server remains stable; strict 400 validation is not required by the supplied contract.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-048.
- **Related defect:** —

## QA-VIS-049 — Verify missing-page handling

- **Preconditions:** API is running.
- **Steps:** 1. Check GET /api/visitors without page; confirm the default response.
- **Expected result:** The endpoint returns a valid default page without error.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-049.
- **Related defect:** —

## QA-VIS-050 — Verify check-in POST API response

- **Preconditions:** Valid registration data and Network panel.
- **Steps:** 1. Check POST for a unique visitor; confirm HTTP status and response fields.
- **Expected result:** HTTP 201 returns the created visitor, host mapping, timestamp, active true and null checkout.
- **Result:** [pass]
- **Actual result / evidence:** User captured POST Response 001 with HTTP 201 and exact JSON.
- **Related defect:** —

## QA-VIS-051 — Verify checkout API response state

- **Preconditions:** An active visitor has an API ID.
- **Steps:** 1. Check PATCH /api/visitors/:id/check_out; confirm checked_out_at and active.
- **Expected result:** The checkout response reports a consistent inactive/checked-out state.
- **Result:** [fail]
- **Actual result / evidence:** Repeated responses retained active:true.
- **Related defect:** DEF-004

## QA-VIS-052 — Verify checked-out visitor leaves the active list

- **Preconditions:** A visitor has a successful checkout.
- **Steps:** 1. Check the active UI immediately after checkout; confirm the row is absent.
- **Expected result:** The checked-out visitor is removed from the active list.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-052.
- **Related defect:** —

## QA-VIS-053 — Verify checkout remains absent after refresh

- **Preconditions:** A visitor has been checked out.
- **Steps:** 1. Check refresh; confirm the same visitor does not return.
- **Expected result:** The active-list query continues to exclude it.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-053.
- **Related defect:** —

## QA-VIS-054 — Verify checkout does not affect unrelated records

- **Preconditions:** At least two active records are visible.
- **Steps:** 1. Check out one record; confirm other rows and pagination remain available.
- **Expected result:** Only the selected record changes.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-054.
- **Related defect:** —

## QA-VIS-055 — Verify immediate checkout after check-in

- **Preconditions:** A newly submitted visitor is visible.
- **Steps:** 1. Check Check Out immediately; confirm no crash and that the row leaves the active list.
- **Expected result:** The newly created visitor can be checked out once.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-055.
- **Related defect:** —

## QA-VIS-056 — Verify meaningful Back/Forward navigation

- **Preconditions:** The current app has a navigable internal history target.
- **Steps:** 1. Check browser Back/Forward; confirm whether a meaningful app state exists.
- **Expected result:** Record N/A when the app exposes no internal history flow.
- **Result:** N/A
- **Actual result / evidence:** Current app has no meaningful internal navigation target.
- **Related defect:** —

## QA-VIS-057 — Verify immediate checkout stability

- **Preconditions:** A newly created visitor and healthy API.
- **Steps:** 1. Check immediate checkout while watching the UI; confirm no freeze or crash.
- **Expected result:** Checkout completes normally.
- **Result:** [pass]
- **Actual result / evidence:** Manual report QA-VIS-057.
- **Related defect:** —

## QA-VIS-058 — Verify browser zoom and layout at 200%

- **Preconditions:** A populated list and registration form.
- **Steps:** 1. Check browser zoom at 200%; confirm controls, rows, headers and Previous/Next remain usable.
- **Expected result:** The UI remains operable without clipped essential controls.
- **Result:** [pass]
- **Actual result / evidence:** User-run 2026-09-17; Page 3/4 navigation remained usable.
- **Related defect:** —

## QA-VIS-059 — Verify pagination remains available after checkout

- **Preconditions:** At least 21 active visitors; Page 1 has a populated Page 2.
- **Steps:** 1. Check out a Page-1 visitor; confirm Next remains enabled while GET /api/visitors?page=2 still has rows.
- **Expected result:** All remaining active visitors stay reachable.
- **Result:** [fail]
- **Actual result / evidence:** Next became disabled while API Page 2 still returned rows.
- **Related defect:** DEF-005

## QA-VIS-060 — Verify deactivated visitors are excluded from the active list

- **Preconditions:** A unique active visitor; deactivation endpoint available.
- **Steps:** 1. Check PATCH /api/visitors/116/deactivate and confirm active:false; refresh and inspect every active page.
- **Expected result:** The deactivated visitor must not appear in the active list.
- **Result:** [fail]
- **Actual result / evidence:** QA Deactivate 060 remained on Page 3 with Check Out.
- **Related defect:** DEF-006

## QA-VIS-061 — Verify deactivated visitors cannot be selected for repeat visits

- **Preconditions:** Visitor 116 has been deactivated.
- **Steps:** 1. Check name search and the registration suggestion; confirm whether the record is selectable.
- **Expected result:** No deactivated visitor appears in search or can autofill the form.
- **Result:** [fail]
- **Actual result / evidence:** QA Deactivate 060 still appeared and autofilled.
- **Related defect:** DEF-007

## QA-VIS-062 — Verify a checked-out visitor can register a repeat visit

- **Preconditions:** Visitor 117 was successfully checked out; its name is searchable.
- **Steps:** 1. Check the name suggestion, confirm company/host autofill, enter Second visit and submit; confirm the old row and new row.
- **Expected result:** The historical checkout remains closed and a new active visit is created.
- **Result:** [pass]
- **Actual result / evidence:** QA Repeat 062: checkout then Second visit row at 16:44.
- **Related defect:** —

## Regression subset — minor registration form update

**Scope assumption:** the change only adjusts registration layout, controlled inputs or validation; it does not intentionally change the API contract, list query, checkout, deactivation or database. If the diff touches a host/search API or shared list component, run the full suite.

| Case | Decision | Rationale |
|---|---|---|
| QA-VIS-001 | Include | Primary submission/payload path is directly affected. |
| QA-VIS-002 | Include | Protects dropdown value-to-ID mapping. |
| QA-VIS-003 | Include | Protects controlled text inputs and payload preservation. |
| QA-VIS-004 | Include | Host initialization can break during form edits. |
| QA-VIS-005 | Exclude | Checkout response state is outside a form-only update; retain in full regression. |
| QA-VIS-006 | Exclude | General pagination navigation is unchanged by the assumed update. |
| QA-VIS-007 | Include | Required-field behavior is a form contract. |
| QA-VIS-008 | Include | Whitespace/name validation is directly affected. |
| QA-VIS-009 | Include | Host-required validation is a form contract. |
| QA-VIS-010 | Include | Registration smoke covers successful submission. |
| QA-VIS-011 | Include | Form reset can regress when submit handling changes. |
| QA-VIS-012 | Include | Submit-button changes can create duplicate requests. |
| QA-VIS-013 | Exclude | No separate search UI exists; repeat suggestions are covered by 061/062. |
| QA-VIS-014 | Include | Registration persistence is a direct form integration check. |
| QA-VIS-015 | Include | Host data loading gates registration. |
| QA-VIS-016 | Include | Input size/layout is directly touched by form changes. |
| QA-VIS-017 | Include | Optional company serialization is part of the form payload. |
| QA-VIS-018 | Include | Text escaping/payload handling can regress in input components. |
| QA-VIS-019 | Include | Optional purpose serialization is part of the form payload. |
| QA-VIS-020 | Include | Form refresh/reset behavior is directly relevant. |
| QA-VIS-021 | Exclude | Checkout action is outside the assumed change. |
| QA-VIS-022 | Exclude | Checkout persistence is server/list behavior, not form-only. |
| QA-VIS-023 | Exclude | Last-page checkout pagination is outside scope. |
| QA-VIS-024 | Include | Normalization is a direct validation concern. |
| QA-VIS-025 | Include | Purpose input layout can be affected. |
| QA-VIS-026 | Include | Company input layout can be affected. |
| QA-VIS-027 | Include | Full Name input layout can be affected. |
| QA-VIS-028 | Include | Safe rendering of submitted input must remain intact. |
| QA-VIS-029 | Include | XSS-safe input rendering is a form safety check. |
| QA-VIS-030 | Include | Registration creates the timestamp subsequently displayed. |
| QA-VIS-031 | Include | Keyboard focus order is part of the form UX. |
| QA-VIS-032 | Exclude | Checkout keyboard action is outside a form-only update. |
| QA-VIS-033 | Include | Name required validation duplicates the form path intentionally. |
| QA-VIS-034 | Include | Keyboard submit can bypass or expose host validation. |
| QA-VIS-035 | Include | Company field mapping is directly touched. |
| QA-VIS-036 | Exclude | Checkout API state is outside scope. |
| QA-VIS-037 | Include | Host selection and registration integration share the form. |
| QA-VIS-038 | Exclude | Same-host checkout is outside scope. |
| QA-VIS-039 | Include | Host assignment smoke catches changed option/value handling. |
| QA-VIS-040 | Exclude | Cross-host checkout is outside scope. |
| QA-VIS-041 | Include | Confirms the edited form payload reaches the displayed/API record. |
| QA-VIS-042 | Exclude | General UI/API pagination is outside scope. |
| QA-VIS-043 | Exclude | Out-of-range API handling is unchanged. |
| QA-VIS-044 | Exclude | Invalid page parsing is unchanged. |
| QA-VIS-045 | Exclude | Page-zero parsing is unchanged. |
| QA-VIS-046 | Exclude | Negative-page parsing is unchanged. |
| QA-VIS-047 | Exclude | Empty-page parsing is unchanged. |
| QA-VIS-048 | Exclude | Decimal-page parsing is unchanged. |
| QA-VIS-049 | Exclude | Default-page API behavior is unchanged. |
| QA-VIS-050 | Include | Direct POST response validates the edited form contract. |
| QA-VIS-051 | Exclude | Checkout response state is outside scope. |
| QA-VIS-052 | Exclude | Checkout list removal is outside scope. |
| QA-VIS-053 | Exclude | Checkout refresh persistence is outside scope. |
| QA-VIS-054 | Exclude | Unrelated-record checkout behavior is outside scope. |
| QA-VIS-055 | Exclude | Immediate checkout is outside scope. |
| QA-VIS-056 | Exclude | Browser history is not a form behavior in this app. |
| QA-VIS-057 | Exclude | Checkout stability is outside scope. |
| QA-VIS-058 | Include | A form layout update must remain usable at zoom. |
| QA-VIS-059 | Exclude | Checkout-driven pagination is outside scope. |
| QA-VIS-060 | Exclude | Deactivation list filtering is server behavior. |
| QA-VIS-061 | Include | Repeat suggestions are a registration-form dependency. |
| QA-VIS-062 | Include | Repeat registration exercises the edited form and suggestion autofill. |

A failure in an included case blocks a clean minor-update result. Exclusion is
not a waiver; excluded cases remain in full-release regression. Policy-dependent
validation is assessed after the Product Owner answers the mandatory-field
question in qa-notes.md.

