# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is the LaTeX source for the STEPSS (Static and Transient Electric Power Systems Simulation) user guide — a formal reference manual covering data formats, model library, installation, and the three integrated modules: Helios (power flow), RAMSES (dynamic simulation), and CODEGEN (user-defined model compiler).

Authors: Dr. Petros Aristidou, Dr. Thierry Van Cutsem.

This *document* is licensed CC BY 4.0 (see LICENSE). The *software* it documents is not, and it is not under a single licence: STEPSS is an umbrella over components licensed separately. The two user interfaces are Apache 2.0; RAMSES is proprietary to the University of Liège, free for non-commercial use; Helios and CODEGEN are under Academic Public Licenses. Do not describe the platform as a whole under any one of these, in either direction. `getting-started/license.md` in stepss-docs owns these facts, `legal.tex` carries the terms printed in the guide, and the README summarises in one paragraph and links out. Anything more here becomes a fourth copy.

The free-of-charge limits (1000 buses, 2 cores) are lifted by a `$LICENSE` record in the user's data files, not by a separate build. There is one RAMSES binary, so nothing that is not the engine can tell which way it is running.

## Build

No Makefile or build scripts exist. Compile with three passes of `pdflatex`: the
first two resolve cross-references, and the guide is long enough that the table
of contents still shifts page numbers on the second.

```bash
pdflatex stepss_doc.tex
pdflatex stepss_doc.tex
pdflatex stepss_doc.tex
```

Output: `stepss_doc.pdf`, about 540 pages. Prerequisites: a LaTeX distribution
with `pdflatex` and the packages listed below.

**Check the log for `\end occurred inside a group`, which is not an error and
does not fail the build.** An unclosed `{\it ...` in `codegen/functions.tex`
leaked slanted roman type into everything that followed it. That file used to be
the last thing included, so it discoloured a few lines and nobody saw it; the
generated chapters now sit after it, and the same bug slanted 350 pages. Any
group left open in an early file does this, silently.

## Document Structure

Main file: `stepss_doc.tex`. It uses sans-serif font globally (`\sf`), A4 custom
margins via `A4.STY`, and `utf8` input encoding. Every hand-written source here
is pure ASCII; the encoding matters because the generated chapters are not, and
`tools/from_docs.py` refuses to emit a character `pdflatex` cannot set.

Eight parts. Anything under `generated/` comes from stepss-docs and is described
in the next section; the rest is hand-written and is the deeper treatment of its
topic.

**Part I, General:** `overview.tex`, `install.tex`, `generated/quickstart`,
`files.tex`, `network.tex`

**Part II, The graphical interface:** `generated/gui-first-run`,
`generated/gui-interface`, `generated/gui-running`

**Part III, Power Flow (Helios):** `power-flow-data.tex`

**Part IV, Dynamic Simulation (RAMSES):** `ref_and_init.tex`,
`sync_thev_impload.tex`, `disturbances.tex`, `solvsett.tex`,
`generated/model-dctl`, `generated/eigenanalysis`

**Part V, Model library:** `generated/model-index` plus eight
`generated/model-*` chapters

**Part VI, User-defined models (CODEGEN):** `user_models.tex`,
`library_blocks.tex` (which includes 42 block files from `codegen/`), then
`generated/codegen-examples` and `generated/cg-studio`

**Part VII, STEPSS in Python:** `generated/py-*`, `generated/uramses`

**Part VIII, Test systems and resources:** `generated/ts-*`, `generated/res-*`

`disc_cont.tex` is gone. It was a 28-line stub on LTC and RT, and
`generated/model-dctl` carries both with their record syntax and worked
examples, plus thirteen further controllers.

## Generated chapters

`tools/from_docs.py` converts stepss-docs pages into `generated/*.tex` with
pandoc. Run it after any change to a page it lists, and `--check` in review to
catch a stale one. The outputs are committed so the guide builds without the
site checked out.

Four things about it are load-bearing:

- **`MANIFEST` holds only pages with no hand-written counterpart here.** Adding
  one that has a counterpart creates the duplication the generator exists to
  avoid.
- **Do not edit `generated/`.** Every file says so in its header, and the next
  run overwrites it.
- **Code fences are masked before anything else runs**, using CommonMark's rule
  of up to three spaces of indent. Most fences on those pages sit inside a list
  and are indented; matching only column-zero backticks pairs an opening fence
  with some later block's closing one, and every transformation past that point
  stops firing without any error.
- **Unicode is mapped, not passed through.** The site draws block diagrams out
  of box-drawing characters and labels them in Greek, and this document is
  pdflatex under OT1. `CODE_MAP` takes the ASCII form inside a fence, where
  verbatim can hold no commands; `TEXT_MAP` takes `\ensuremath{...}` outside
  one, which is legal in both modes. The mapping runs on the finished LaTeX,
  after pandoc has decided what is math: doing it to the Markdown puts a dollar
  next to one pandoc has not resolved and it pairs the wrong two.

## Key Directories

- `codegen/`, 42 `.tex` files each documenting one CODEGEN modelling block (transfer functions, integrators, PI controllers, limiters, FSAs, etc.), plus corresponding PDF block diagrams. These are `\input`-ed by `library_blocks.tex`.
- `models/`, reference PDF documentation for specific device models (wind turbines, thermal torque controllers, discrete controllers).
- `generated/`, chapters mirrored from stepss-docs. Generated; do not edit.
- `figures/`, images those chapters reference, collected by the generator. SVG sources are converted to PDF on the way in. Generated; do not edit.
- `tools/`, the generator.

## LaTeX Conventions

- **Custom page style**: `A4.STY` sets 165mm text width, 230mm text height, 1.1 baseline stretch, zero paragraph indent with 3ex paragraph skip.
- **Key packages**: `amsmath`, `mdframed`, `algorithmic`, `fontawesome5` (for `\faHandPointRight`), `hyperref`, `enumitem` (with `nolistsep`), `epsfig`, `bm`, `xcolor`. The generated chapters add `longtable`, `booktabs`, `array`, `calc`, `graphicx` and `fvextra`, all loaded from `docs-preamble.tex` along with the `docsnote` box and the `hypersetup` that colours links instead of boxing them.
- **Cross-references for blocks**: CODEGEN block sections use `\hypertarget{blockname}` / `\hyperlink{blockname}` for internal linking between block descriptions.
- **Figures**: All diagrams are PDF or PNG format, included at the repo root level, in `codegen/` and in `figures/`.
- **Data format documentation**: Network and power-flow records are documented with `tabular` environments showing field-by-field breakdowns. Each record type is semicolon-terminated, matching the `.dat` file format consumed by RAMSES and Helios.
