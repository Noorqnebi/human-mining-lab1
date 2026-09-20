# Human Mining Lab — attention protocol v0.2

The existing research application, UI, network simulator and finite reward engine are retained. HMC Simulation remains virtual. A separate native HMC-Test layer is documented in [the private testnet runbook](testnet/README.md); it does not migrate these simulation balances. These experiments measure attention, not unique humanity or security against a malicious client.

## Defaults and scheduling

Sessions offer 10, 20 (default), 30 and 60 minutes. Founder Console exposes five principal attention controls: frequency bounds 20–28 seconds, start window 4 seconds, continuous proportion 40%, Medium miss penalty, attention weight 50%. Existing economic settings remain available. Each session snapshots its settings; changes apply to subsequent sessions.

Scheduling uses fresh browser cryptographic randomness, never a fixed interval or a repeating cycle. An irregular mixture targets a long-run onset interval within 20–28 seconds, with occasional approximately 6–12 second post-completion bursts and usually a 32–40 second recovery gap after a burst. Sampled sessions can deviate from the population mean. A pending challenge is never overlapped in the same window. Expired deadlines are caught up using their original times after timer throttling; returning to a window cannot reopen a missed challenge.

An invitation must be started strictly before its 4-second deadline. Starting at or after that deadline is rejected in both UI and engine. Missing an invitation closes that challenge, not the entire session. Successful execution requires a complete timed interaction trace, not merely clicking Begin.

## Interaction mix

Ordinary tasks occupy 5–12 seconds (memory 8–12), with two timed steps. Continuous tasks make up 40%. The remaining 60% is distributed across target, direction, memory, dynamic selection and hold using their existing relative weights. Immediate repeats receive less selection weight without imposing a repeating cycle. Six continuous variants cover tracking, hold-and-adjust, mixed action sequences, changing patterns, changing color cues and alternating targets. Mixed sequences draw four distinct simple actions; selection tasks may use circle size or a clearly named color. They occupy 12–20 seconds. Tracking samples must cover at least 72% of the task; step tasks need at least 75% of steps, rounded up. No maths, trivia or cognitive difficulty escalation is used.

Instructions and targets are deliberately large. Targets move smoothly; continuous tasks need ongoing pointer/touch movement, timed holds or successive actions. Losing focus releases a held interaction.

## Measured work and efficiency

VHT is focused time supported by a completed verification. A successful verification credits focused seconds since the preceding completed challenge. Failed intervals receive no credit. A short tail after a successful verification is provisionally credited, capped at 70 seconds under defaults. Unfocused time and browser suspension gaps do not accrue VHT.

Let T = VHT / elapsed, S = successful challenges / completed challenges, Q = mean successful interaction quality, R = focused fraction × exp(-0.0075 × focus-loss count).

Default contributions are:

- Time: 0.50 × T.
- Challenge: 0.30 × S.
- Quality/reliability: 0.20 × (0.70 × Q + 0.30 × R).

Efficiency = sum of contributions × missed-start multiplier, clamped to 0–1; no successful work yields zero efficiency. HU = VHT × efficiency × the existing HU-per-second and difficulty-weight settings. Changing attention weight assigns the remainder 60:40 to success and quality/reliability.

The smooth multiplier is exp(-k × missedStarts^2.3), where k is .0015 Light, .003 Medium or .005 Strong. Under Medium, 1, 2, 3, 4 and 5 misses reduce the pre-penalty efficiency by approximately 0.30%, 1.47%, 3.68%, 7.02% and 11.45%. A missed interval also reduces verified time and success. Started but failed tasks reduce success/quality without incrementing the missed-start count. There is no arbitrary whole-session rejection for a single miss.

Attention Score = 60% verified-time fraction + 25% non-missed-start fraction + 15% continuous-task quality. If no continuous task has occurred, the last component uses overall success. This diagnostic shares inputs with efficiency; it is not multiplied into rewards a second time.

## Focus and parallel windows

Visibility and window focus transitions flush elapsed time before changing the focus state. Focus loss stops accrual immediately. Background events, total unfocused/suspended time and focus-loss events are stored. Focus recovery continues the active session. A page reload offers interrupted-session recovery without issuing new rewards for unverified work.

No device fingerprint, IP restriction, cross-window mining lock or one-tab prohibition is present. Existing club time quotas are not enforced in attention research sessions so the requested 20–60 minute experiments can run; club multipliers and network simulation assumptions are preserved.

IndexedDB stores each session and ledger entry separately. Only the brief reward settlement transaction is serialized, preventing duplicate issuance and concurrent-window overwrite. Settings changes propagate across windows; each active session retains its snapshot. Existing localStorage history and wallet records are imported without recalculation, and the original legacy data is retained. Data remains browser/origin local and does not sync across devices. Do not reset data to test an upgrade.

## Proof and rewards

New challenge commitments include immutable duration, start deadline and generated frames. Response commitments include start time, status, interaction trace and verified focus interval. Legacy proofs retain their original verification algorithm. Client-side hashes detect inconsistent records but cannot prevent a person controlling the client from forging an internally consistent record.

Accepted sessions require chain integrity, configured minimum duration/count, some successful work and nonzero HU. Old success-ratio and identical-timing whole-session rejection gates do not apply to attention sessions.

The existing reward engine apportions weighted HU across UTC epochs against simulated network HU. Remaining epoch pools, daily caps, session caps and lifetime issuance limits still apply. Club bonuses retain separate ledger entries. Settlement is idempotent by session ID. Simulations and test credits do not masquerade as mined work.

## Research reporting

Session results include density, average onset interval, bursts, misses, continuous completions, focus losses, background events/time, Attention Score, Reliability, contribution breakdown, final efficiency, HU and HMC.

Research Lab aggregates measured sessions. Attention integrity, density, miss rate, focus-loss rate and human attention load derive from recorded observations. Parallelization risk is an explicitly experimental weighted heuristic, not a security claim and not a reward modifier. Multi-session efficiency groups actual overlapping sessions (1, 2, 3 and 5+ windows); absent groups say Not tested. There is no programmed exponential decline or forced output reduction based on account count.

## Validation

Run `node tests/protocol.test.mjs`. Tests cover a simulated 600-second focused session, a large cadence sample, exact start-deadline rejection, all six continuous modes and durations, early completion rejection, focus loss and resume, progressive penalties, live configuration effects, trace tamper rejection, legacy migration/proof compatibility, HU/reward caps and absence of an artificial parallel-account efficiency penalty.

The automated timing simulation does not establish a human comfort or success-rate baseline. Single-user 90%+ success and 85–100% efficiency remain experimental targets. Compare actual 1-, 3- and 5-window trials with identical settings, and inspect the measured results without assuming the hypothesis is true.

## Denser attention profile

The follow-up profile increases density and variety without shortening the four-second start window, shrinking touch targets or increasing memory complexity. Defaults are 20–28 seconds and 40% continuous. The default verification cap is 240 so long sessions keep receiving challenges. Existing sessions retain their snapshots; old default settings upgrade once while custom cadence and continuous percentages are retained. Measured population cadence is approximately 24.77 seconds with completed work and 22.30 seconds with missed invitations. These are timing simulations, not human performance claims.

## Expanded challenge library

Ten independently selectable continuous practice modes now include drag-and-deliver, slider alignment, two-beat follow-up and spotlight grid, alongside the six existing modes. Mining selects from this same library. Continuous tasks randomly use 3–5 steps within the unchanged 12–20 second duration. Mixed sequences draw distinct actions, signal onset varies, target positions vary, and modes seen among the last three challenges have reduced (not zero) probability. There is no fixed rotation.

The existing denser cadence was calibrated against 1,000 synthetic five-minute runs: mean 11.766 challenges, 97.1% of samples within 10–14. This is a stochastic target, not a hard count guarantee in every rolling five-minute window. Custom Founder cadence settings override the defaults.
