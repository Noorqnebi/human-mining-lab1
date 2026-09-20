# Phase 1 package validation

This is the current Human Mining Lab source with local-launcher readiness checks, not a separate application and not a Cosmos SDK migration.

- Real Linux process smoke: the exact `node testnet/scripts/start-local-lab.mjs` command initialized a fresh zero-supply Legacy chain, started three CometBFT validators, started Oracle and frontend, served HTML and `/api/testnet/state`, observed advancing block height, and stopped on SIGTERM. No mining session or mint was fabricated.
- The smoke used the available precompiled Linux node and installed Node dependencies. Neither is shipped. The recipient must run npm ci; first launch builds the native Go node on their Mac.
- Repeated the launcher smoke from the ZIP extracted into a clean temporary directory without any .env.local or legacy secrets. Test-only links to installed dependencies and the Linux executable were added outside the ZIP. The extracted source reached READY and passed the same checks.
- Frontend build, application TypeScript check, JavaScript golden vectors, wallet encryption and research-report regression tests passed.
- macOS was not available here. Clean macOS Go compilation and real human browser acceptance remain recipient acceptance steps. A prerequisite check does not prove compilation.
- No browser wallet private key, passphrase, Oracle/admin/validator key or prior ledger is included. Runtime secrets are generated locally on first start. Prior result JSON files are historical evidence, not state imported into this fresh chain.
- ZIP CRC and per-file SHA-256 manifest are verified during packaging. The payload is selected from source files, excluding dependencies, builds, caches, generated executables, environment secrets and runtime data. It is also compared against the known local testnet secret values without exporting them.
- HMP/0.1 JavaScript golden signature/commitment tests and encrypted wallet/report regressions are supplied. Go golden fixture tests are supplied; this packaging run does not assert a successful fresh Go compilation.
- Human Browser → Native Mint acceptance is intentionally pending the user's local session. Follow README-macOS.md and return the public acceptance export.
