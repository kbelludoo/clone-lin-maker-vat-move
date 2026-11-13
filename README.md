# clone-lin-maker-vat-move

Experimental **LIN** clone of MakerDAO / Sky `Vat.move` — the internal DAI
(rad) transfer kernel (sequential unsigned `_sub` then `_add` of `dai[src]`
and `dai[dst]` after src-only `wish`).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [makerdao/dss](https://github.com/makerdao/dss) `fa4f6630afb0624d04a003e920b0d71a00331d98`
  - `src/vat.sol` sha256 `9016fd94f0f1b014abf184edab86a3f5843042b17e3734407e0f0fabb920f49f` blob `ff90bbed5b36e54318bd5461cbf32e4167df2b28`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_maker_vat_move.lin`](src/lin_maker_vat_move.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/maker_vat_move_c11.c`](test/oracles/maker_vat_move_c11.c)
- **Proofs (lin-open only):** `examples/maker_vat_move/` and `make maker-vat-move-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256. Not a production vat replacement.

Named goldens: SAFE_XFER `src=5e9 dst=3e9 rad=1e9 wish=1` → **move_ok=1**, leftovers `4e9/4e9`, conserved;
SELF_MOVE `src=dst=7e18 rad=3e18` is identity under sequential sub-then-add while naive parallel `add_ok(7e18,3e18)=0`;
NO_WISH → **0**; UNDERFLOW rad>src → **0**; CROSS OVERFLOW `src=3e18 dst=7e18 rad=3e18` → **0**;
zero rad is a no-op **ok=1**; full drain src→0 dst=8e9; no `live`, no `wards` (src-only wish);
unit is rad (not gem wad); naive i64 `7e18+3e18` wraps, checked u_add fail-closes.

## Reproduce proofs (in lin-open)

```bash
make maker-vat-move-proof
# or: python3 test/prove_maker_vat_move_external.py
```

## Not claimed

Uint256 / mainnet dai-rad parity.
LIN replacing Maker, Sky, vat, or the EVM. TVL or dollar savings. zk soundness.
`wish` mappings as hidden storage. `vat.flux` (gem wad; see clone-lin-maker-vat-flux).
