# Attention update validation — 2026-09-05

- Application TypeScript check: passed.
- Production build: passed (existing chunk-size warning only).
- Protocol suite: passed A–G.
- Final synthetic 600-second focused session: 19 completed challenges, mean onset 30.13 seconds, efficiency 99.99%. This uses generated ideal input traces and is not a human usability measurement.
- 20,000-interval population: mean 30.164 seconds with executed tasks; 28.241 seconds with missed invitations. Both include bursts and quiet gaps.
- Strict four-second start boundary, early-completion rejection, all four 12–20 second continuous variants, focus transition accounting, escalating penalties and all five Founder controls: tested in engine.
- HU, finite reward allocation, supply limits, trace tamper rejection and unchanged legacy proof/balance migration: passed.
- Browser: a full unattended 10-minute run completed and persisted, with zero work/reward. It exposed overly dense missed-only scheduling (21.7-second sample mean), which was corrected and revalidated by the population and 600-second tests.
- Browser after correction: a second 10-minute run reached over six minutes; a real two-step selection passed, generating 35 seconds VHT and positive provisional HU/HMC. The cloud browser was replaced between conversation turns before final settlement could be inspected. Final browser settlement for this run is therefore unverified.
- Browser: continuous tracking and dynamic sequence tasks visibly occupied 17–19 seconds; unchanged dark UI inspected.
- Browser: changing Founder response window to 8 seconds allowed starting a practice after 5 seconds and completing two target steps. Restored to 4 seconds. The already-active mining session retained its original 4-second snapshot.
- Browser: late invitation button disappeared and could not be clicked after expiry. The session continued.
- Browser: saved history survived reload and was visible from a second tab.
- Background exclusion is covered by engine transition tests. This cloud browser did not produce real focus/visibility changes when switching its controlled tabs, so physical background-tab behavior still needs a user-device check.

Remaining human validation: one fully attended 10-minute session on desktop and phone; background/focus behavior on the user's browser; then comparable 1-, 3- and 5-window trials. Do not treat ideal synthetic efficiency as a measured human success/comfort baseline. Concurrent ledger settlement is transactional in IndexedDB but was not independently stress-tested in the browser.

## Follow-up: density and variety

- Defaults: cadence bounds 20–28 seconds, continuous 40%, six continuous variants, cap 240. Start window remains 4 seconds and task durations/acceptance thresholds are unchanged.
- Updated protocol suite and TypeScript check passed. Population means: 24.774 seconds with executed tasks and 22.304 seconds with missed starts. Synthetic 10-minute sample: 23 completions, 25.05-second mean onset.
- All six continuous modes stay within 12–20 seconds. Mixed action sequences contain four distinct simple actions, including color and delayed pattern interaction.
- Migration tests verify old defaults upgrade and custom cadence/continuous settings survive. Legacy proof and reward checks still pass.
- Follow-up changes were checked in code and the production build; no new browser or human usability run is claimed.

## Expanded library validation

Engine tests cover all ten modes with successful traces and rejection of empty traces; variable 3–5 step generation and old proof/reward compatibility pass. A 1,000-session five-minute timing simulation produced mean 11.766 challenges and 97.1% within 10–14. The new pointer/touch controls have TypeScript/build validation, not a new browser or phone usability test.
