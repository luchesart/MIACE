# MIACE — Lean formalization and proof map

Lean formalization accompanying Paper 13, *MIACE and Monotone Tracking: Exact Tuning for First-Order Plants and Positive Cascades with Pure Delay*, including the nonlinear first-order storage-balance extension.

Version **0.20.0**, corresponding to the author-edited mathematical text of **22 October 2026**.

The reviewed map covers **32 numbered statements**, with **1,227 mapped declarations** across **199 proof modules**. The project contains 201 Lean files including the aggregate import and axiom audit.

## Read the map

- [Theorem index](docs/PROOF_MAP.md): a GitHub-readable table linking paper statements to their Lean entry points.
- [Interactive proof map](docs/MIACE_Proof_Map.html): download and open in a browser. It works offline, with searchable statements, paper proof excerpts, Lean proofs, hypotheses and correspondence notes.
- [Machine-readable map](docs/proof-map.json).

The complete LaTeX manuscript, PDFs, old project snapshots and build caches are excluded. Manuscript filenames and line numbers in the map identify the captured source; the relevant excerpts are embedded in the map itself.

## Build

Install Lean through `elan` and ensure `lake` is on your PATH. Run these commands from the repository root:

```sh
lake exe cache get
lake build
lake env lean Audit.lean > audit.log 2>&1
python3 tools/check_audit.py audit.log
python3 tools/check_package.py
```

Use Python 3.9 or newer for the helper scripts. The first command downloads dependencies and mathlib's compiled cache, so it requires internet access. Subsequent builds use the local cache.

The versions are pinned in `lean-toolchain`, `lakefile.toml` and `lake-manifest.json`:

- Lean **4.34.0**
- mathlib **5ed2965256430c3649e86755f9576b54eca72435**

Keep the manifest under version control. `lake update` can change dependency resolution and is not part of reproducing this release.

## Verification and scope

The full version 0.20.0 build passed 3,759 build jobs. Its axiom audit covered all 1,227 mapped declarations and permitted only `propext`, `Classical.choice` and `Quot.sound`. [Verification metadata](docs/verification.json) records provenance and exact Lean file hashes. Every Lean source file and dependency pin in this reduced package is unchanged from that verified version. The reduced package was also successfully built using the existing caches, and its 1,227-declaration axiom audit passed again.

The map's completion status records review of the formal statements against the paper. Lean checks the formal proofs; it does not itself prove equivalence between prose and code. The map documents differences in proof routes. For example, Theorem 32 constructs existence through an inverse clock instead of the paper's Picard–Lindelöf and continuation argument. It covers the stated scalar nonlinear case with zero delay.

`check_package.py` checks the map against the distributed source files and checksums. It does not invoke the Lean kernel; run the build and axiom audit above for that check.

## Repository layout

| Path | Purpose |
| --- | --- |
| `MIACE/` | Proof modules and definitions |
| `MIACE.lean` | Aggregate import of all proof modules |
| `Audit.lean` | Axiom audit for every mapped declaration |
| `docs/` | Proof maps and verification metadata |
| `tools/` | Map rendering and consistency/audit checks |
| `lean-toolchain`, `lakefile.toml`, `lake-manifest.json` | Reproducible toolchain and dependencies |

To regenerate the HTML after editing the JSON map:

```sh
python3 tools/render_map.py
```

An optional UI logic check requires Node.js:

```sh
node tools/check_map_ui.cjs
```

This checks search, status filters and embedded data; it is not a visual browser test. Its generated `map-qa.json`, the axiom log and Lake's cache are ignored by Git.

For GitHub, extract this archive and upload the contents of its `MIACE_Lean_GitHub` folder as the repository contents, keeping the directory structure.
