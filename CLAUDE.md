# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is the LaTeX source for the STEPSS (Static and Transient Electric Power Systems Simulation) user guide — a formal reference manual covering data formats, model library, installation, and the three integrated modules: PFC (power flow), RAMSES (dynamic simulation), and CODEGEN (user-defined model compiler).

Authors: Dr. Petros Aristidou, Dr. Thierry Van Cutsem.

This *document* is licensed CC BY 4.0 (see LICENSE). The *software* it documents is not: STEPSS is under the Academic Public License (non-commercial), whose terms are reproduced in `legal.tex`. Don't conflate the two.

## Build

No Makefile or build scripts exist. Compile with two passes of `pdflatex` to resolve cross-references:

```bash
pdflatex stepss_doc.tex
pdflatex stepss_doc.tex
```

Output: `stepss_doc.pdf`. Prerequisites: a LaTeX distribution with `pdflatex` and the packages listed below.

## Document Structure

Main file: `stepss_doc.tex`. It uses sans-serif font globally (`\sf`), A4 custom margins via `A4.STY`, and `latin1` input encoding.

**Part I — General (all modules):**
1. `overview.tex` — Quick overview
2. `install.tex` — Installation
3. `files.tex` — Data file organisation
4. `network.tex` — Network component modelling

**Part II — Power Flow (PFC):**
5. `pfc-data.tex` — PFC data format

**Part III — Dynamic Simulation (RAMSES):**
6. `ref_and_init.tex` — Reference frame and initialization
7. `sync_thev_impload.tex` — Synchronous machines, Thévenin equivalents, impedance loads
8. `disturbances.tex` — Disturbances
9. `solvsett.tex` — Solver settings
10. `disc_cont.tex` — Discrete controllers

**Part IV — User-defined models (CODEGEN):**
11. `user_models.tex` — CODEGEN mathematical formulation and DSL syntax
12. `library_blocks.tex` — Library of modelling blocks (includes 42 individual block files from `codegen/`)

## Key Directories

- `codegen/` — 42 `.tex` files each documenting one CODEGEN modelling block (transfer functions, integrators, PI controllers, limiters, FSAs, etc.), plus corresponding PDF block diagrams. These are `\input`-ed by `library_blocks.tex`.
- `models/` — Reference PDF documentation for specific device models (wind turbines, thermal torque controllers, discrete controllers).

## LaTeX Conventions

- **Custom page style**: `A4.STY` sets 165mm text width, 230mm text height, 1.1 baseline stretch, zero paragraph indent with 3ex paragraph skip.
- **Key packages**: `amsmath`, `mdframed`, `algorithmic`, `fontawesome5` (for `\faHandPointRight`), `hyperref`, `enumitem` (with `nolistsep`), `epsfig`, `bm`.
- **Cross-references for blocks**: CODEGEN block sections use `\hypertarget{blockname}` / `\hyperlink{blockname}` for internal linking between block descriptions.
- **Figures**: All diagrams are PDF or PNG format, included at the repo root level and in `codegen/`.
- **Data format documentation**: Network/PFC records are documented with `tabular` environments showing field-by-field breakdowns. Each record type is semicolon-terminated, matching the `.dat` file format consumed by RAMSES/PFC/Helios.
