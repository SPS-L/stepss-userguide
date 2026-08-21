# STEPSS User Guide

**LaTeX source of the STEPSS documentation of models and user guide.**

This repository holds the LaTeX sources, figures, and compiled PDF of the official user documentation: a models reference and user guide covering the Helios, RAMSES, and CODEGEN modules, the two user interfaces and the model library, part of the [STEPSS](https://stepss.sps-lab.org/) power system simulation platform.

The guide and [the documentation site](https://stepss.sps-lab.org/) carry the same material. Chapters that exist on both are generated from the site's Markdown by `tools/from_docs.py` rather than written twice; see [Generated chapters](#generated-chapters).

## Building the Documentation

There is no Makefile or `latexmk` configuration. Compile with two passes of `pdflatex` (the second pass resolves cross-references):

```bash
pdflatex stepss_doc.tex
pdflatex stepss_doc.tex
```

A third pass is worth running: the guide is long enough that the table of contents shifts page numbers on the second.

Requirements: any LaTeX distribution providing `pdflatex` and a full TeX Live.
The document class `sps-report.cls` is in the repository and pulls in what it
needs; the notable ones are `newpx` (Palatino) for the body, `roboto` for the
titles, `titlesec`, `titletoc`, `fancyhdr`, `tikz`, `caption`, `microtype`,
`siunitx` and `cleveref`. `docs-preamble.tex` adds what the generated chapters
need on top: `fvextra`, `xurl`, `hyphenat`, `array` and `calc`.

One pre-built PDF is committed: `stepss_doc.pdf`, the build output of
`stepss_doc.tex`.

After rebuilding, copy it to `public/stepss_docs.pdf` in
[stepss-docs](https://github.com/SPS-L/stepss-docs), which is what the website
serves at [/stepss_docs.pdf](https://stepss.sps-lab.org/stepss_docs.pdf). That
is the only downstream copy.

## Project Structure

```
stepss_doc.tex        the document: parts, chapters, and nothing else
sps-report.cls        the SPS-L report class
docs-preamble.tex     what the generated chapters need on top of the class
frontmatter/          legal.tex, the licence printed at the front
mainmatter/           the hand-written chapters
mainmatter/codegen/   one .tex per CODEGEN modelling block
generated/            chapters mirrored from stepss-docs; do not edit
figures/              every image, including figures/codegen/ block diagrams
models/               standalone companion notes for individual device models
tools/                the generator
```

The guide is in eight parts:

| Part | Covers | From |
|---|---|---|
| I | All modules: overview, installation, quick start, data files, network | `mainmatter/` + `generated/quickstart` |
| II | The graphical interface | `generated/gui-*` |
| III | Power flow with Helios | `mainmatter/power-flow-data.tex` |
| IV | Dynamic simulation with RAMSES | `mainmatter/` + `generated/model-dctl`, `generated/eigenanalysis` |
| V | Model library | `generated/model-*` |
| VI | User-defined models with CODEGEN | `mainmatter/user_models.tex`, `mainmatter/library_blocks.tex`, `mainmatter/codegen/`, `generated/codegen-examples`, `generated/cg-studio` |
| VII | STEPSS in Python | `generated/py-*`, `generated/uramses` |
| VIII | Test systems and resources | `generated/ts-*`, `generated/res-*` |

Every `\includegraphics` resolves through `\graphicspath{{figures/}{./}}`, so a
figure is referenced by name wherever the chapter including it happens to sit.

## Generated chapters

Everything under `generated/` is written by `tools/from_docs.py` out of the
Markdown in a `stepss-docs` checkout sitting beside this one. Do not edit those
files: edit the page named in each one's header comment, then

```bash
./tools/from_docs.py            # rewrite generated/ and refresh figures/
./tools/from_docs.py --check    # fail if anything is stale
```

The outputs are committed, so the guide builds with no checkout of the site
present. Hand-written chapters are never touched by the generator, and its
`MANIFEST` lists only pages that have no hand-written counterpart here: the
data-format and CODEGEN-block chapters remain the authoritative, deeper
treatment and are not generated from anything.

Figures are collected the same way. `figures/` mirrors the images those pages
reference, with SVG sources converted to PDF; the screenshots come from the
capture harness in `stepss-docs/tools/`.

## What is STEPSS?

**STEPSS** (*Static and Transient Electric Power Systems Simulation*) is a power system simulation tool for dynamic studies of electrical grids. It performs power flow computations and simulates the dynamic response of power systems to disturbances under the phasor approximation.

STEPSS consists of three tightly integrated modules:

| Module | Full Name | Description |
|--------|-----------|-------------|
| **Helios** | AC Power Flow | Determines the initial operating point using the Newton-Raphson method in polar coordinates. Computes bus voltage magnitudes and phase angles, with optional transformer ratio adjustment. |
| **RAMSES** | RApid Multithreaded Simulation of Electric power Systems | Simulates the dynamic evolution of the power system. Supports Backward Euler, Trapezoidal, and BDF2 integration methods. Exploits OpenMP parallelism (up to 2 cores in the free version). |
| **CODEGEN** | CODE GENerator | Translates user-defined models from text descriptions into Fortran 2003 code for compilation and linking. Supports excitation controllers, torque controllers, injectors, and two-port components. |

## Prerequisites (running STEPSS)

The Java GUI documented in this guide comes two ways, both on the [releases page](https://github.com/SPS-L/stepss-java-ui/releases):

- an **installer** (`.msi`, `.dmg` or `.deb`) that carries its own Java, so nothing else is needed
- **`stepss.jar`**, which needs a 64-bit **Java Runtime Environment, version 11 or later** already installed, and self-extracts at startup

To compile user-defined models (CODEGEN), the toolchain is **GNU Fortran**, because the URAMSES kit STEPSS carries is a gfortran build:

- **Windows**: MSYS2, then `pacman -S mingw-w64-x86_64-gcc-fortran mingw-w64-x86_64-openblas make`
- **Linux** (Debian, Ubuntu): `sudo apt install gfortran make libopenblas-dev`
- **macOS**: `brew install gcc openblas`

Intel oneAPI (`ifx`) and the Visual Studio environment it needs are no longer part of this. Every RAMSES binary distributed today, on all three platforms, is a gfortran build.

STEPSS is not Windows-only. RAMSES also builds on Linux and macOS with gfortran, and stepss installs with `pip` on Windows and Linux. See [stepss.sps-lab.org](https://stepss.sps-lab.org/).

## Limitations (free academic version)

- Maximum **1000 buses/nodes**
- Maximum **2 processor cores** for parallelisation

## Documentation

- **Published guide (PDF)**: [stepss.sps-lab.org/stepss_docs.pdf](https://stepss.sps-lab.org/stepss_docs.pdf)
- **Documentation website**: [stepss.sps-lab.org](https://stepss.sps-lab.org/), the same content published as a website, built from the [stepss-docs](https://github.com/SPS-L/stepss-docs) repository

## License

This guide (the LaTeX sources, the figures, and the built PDF) is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license. See [LICENSE](LICENSE). Share and adapt it freely, including commercially, with appropriate credit.

**The software it documents is not, and it is not under one licence.** STEPSS is an umbrella over components licensed separately: the two user interfaces are Apache 2.0, while the engines are not. RAMSES is the property of the University of Liège (Belgium) and is proprietary, free for non-commercial use; Helios and CODEGEN are under Academic Public Licenses. The free-of-charge terms limit a model to 1000 buses and parallelisation to 2 cores, lifted by a `$LICENSE` record in the data files rather than by a different build.

[**Licensing**](https://stepss.sps-lab.org/getting-started/license/) on the documentation site is the single owner of these facts, and the terms printed at the front of the guide come from `legal.tex`. Summarising them here beyond the paragraph above only creates a second copy to drift: this file previously described the whole of STEPSS as Academic Public License, which is one component's terms applied to the platform.

See [NOTICE](NOTICE).

## Authors

Developed and maintained by the [Sustainable Power Systems Laboratory (SPS-L)](https://sps-lab.org/) at the Cyprus University of Technology, under the direction of Dr. Petros Aristidou.

The guide is authored by:

- **Dr. Petros Aristidou**: petros.aristidou@cut.ac.cy
- **Dr. Thierry Van Cutsem**: thierry.h.van.cutsem@gmail.com
