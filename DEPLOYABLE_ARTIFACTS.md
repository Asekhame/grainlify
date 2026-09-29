# Deployable Artifacts Inventory

This document lists every deployable WebAssembly (wasm) artifact produced by this repository, its source crate, and its workspace.

Only artifacts in the first table may be deployed. Crates whose wasm appears under [Not deployable](#not-deployable) are built and tested in CI but must never be deployed to any network.

Where a contract exists as two same-name crates in different workspaces, exactly one of them is authoritative:

- **Escrow** — the deployed escrow is `contracts/bounty_escrow/contracts/escrow` (`bounty_escrow.wasm`). See [`docs/contracts/escrow-implementation-authority.md`](docs/contracts/escrow-implementation-authority.md).
- **Program escrow** — the deployable program escrow is `contracts/program-escrow` (`program_escrow.wasm`). See [`docs/contracts/program-escrow-implementation-authority.md`](docs/contracts/program-escrow-implementation-authority.md).

## Deployable artifacts

| Artifact | Source Crate | Workspace | Status |
|---|---|---|---|
| `bounty_escrow.wasm` | `contracts/bounty_escrow/contracts/escrow` | `contracts/bounty_escrow` | Deployed |
| `grainlify_core.wasm` | `contracts/grainlify-core` | `contracts` | Deployed |
| `escrow_view_facade.wasm` | `contracts/escrow-view-facade` | `contracts` | Staged |
| `program_escrow.wasm` | `contracts/program-escrow` | `contracts` | Staged |
| `view_facade.wasm` | `contracts/view-facade` | `contracts` | Staged |

"Staged" means the artifact is deployable but no network deployment is recorded in this repository. Per the root `README.md`, a missing network ID means a live deployment is unverified here, not that one has never existed.

## Not deployable

These crates produce a wasm because they declare `crate-type = ["cdylib"]`, and `scripts/check_inventory.py` therefore requires their artifact names to be listed here. They are **not** deployable and must never be deployed.

| Artifact | Source Crate | Workspace | Status | Authority |
|---|---|---|---|---|
| `escrow.wasm` | `soroban/contracts/escrow` | `soroban` | Superseded — reference/parity only | `bounty_escrow.wasm` from `contracts/bounty_escrow/contracts/escrow` |
| `soroban_program_escrow.wasm` | `soroban/contracts/program-escrow` | `soroban` | Superseded — reference only | `program_escrow.wasm` from `contracts/program-escrow` |

Neither superseded crate has a size budget entry in `.github/wasm-budgets.json`, and no deploy script targets either artifact.
