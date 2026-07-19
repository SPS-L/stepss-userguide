# STEPSS User Guide

**LaTeX source of the STEPSS documentation of models and user guide.**

This repository holds the LaTeX sources, figures, and compiled PDF of the official user documentation — a models reference and user guide covering the PFC, RAMSES, and CODEGEN modules — part of the [STEPSS](https://stepss.sps-lab.org/) power system simulation platform.

## Building the Documentation

There is no Makefile or `latexmk` configuration. Compile with two passes of `pdflatex` (the second pass resolves cross-references):

```bash
pdflatex stepss_doc.tex
pdflatex stepss_doc.tex
```

Requirements: any LaTeX distribution providing `pdflatex` and the packages `times`, `epsfig`, `bm`, `hyperref`, `enumitem`, `algorithmic`, `fontawesome5`, `amsmath`, and `mdframed`. The custom page style `A4.STY` is included in the repository.

Two pre-built PDFs are committed, and they are kept identical:

- `stepss_doc.pdf` — the build output of `stepss_doc.tex`, and the source of the copy published at [stepss.sps-lab.org](https://stepss.sps-lab.org/).
- `userguide.pdf` — the same document under the file name the Java GUI expects. It is bundled, together with the companion notes in `models/`, into `DOC.zip` in [stepss-java-ui](https://github.com/SPS-L/stepss-java-ui) and extracted at run time.

After rebuilding, refresh both copies, then regenerate `DOC.zip` in `stepss-java-ui` and update `public/stepss_docs.pdf` in `stepss-docs` so the GUI and the website ship the current guide.

## Project Structure

The main entry point is `stepss_doc.tex`, which organises the guide in four parts:

- **Part I — All modules**: `overview.tex`, `install.tex`, `files.tex`, `network.tex`
- **Part II — Power Flow (PFC)**: `pfc-data.tex`
- **Part III — Dynamic Simulation (RAMSES)**: `ref_and_init.tex`, `sync_thev_impload.tex`, `disturbances.tex`, `solvsett.tex`, `disc_cont.tex`
- **Part IV — User-defined models (CODEGEN)**: `user_models.tex`, `library_blocks.tex`, plus `codegen/` (one `.tex` file per modelling block, with block-diagram PDFs)

Other notable files: `legal.tex` (the software license printed at the front of the guide), `models/` (companion PDF notes for specific device models, bundled with the GUI).

## What is STEPSS?

**STEPSS** (*Static and Transient Electric Power Systems Simulation*) is a power system simulation tool for dynamic studies of electrical grids. It performs power flow computations and simulates the dynamic response of power systems to disturbances under the phasor approximation.

STEPSS consists of three tightly integrated modules:

| Module | Full Name | Description |
|--------|-----------|-------------|
| **PFC** | Power Flow Computation | Determines the initial operating point using the Newton-Raphson method in polar coordinates. Computes bus voltage magnitudes and phase angles, with optional transformer ratio adjustment. |
| **RAMSES** | RApid Multithreaded Simulation of Electric power Systems | Simulates the dynamic evolution of the power system. Supports Backward Euler, Trapezoidal, and BDF2 integration methods. Exploits OpenMP parallelism (up to 2 cores in the free version). |
| **CODEGEN** | CODE GENerator | Translates user-defined models from text descriptions into Fortran 2003 code for compilation and linking. Supports excitation controllers, torque controllers, injectors, and two-port components. |

## Prerequisites (running STEPSS)

The Java GUI documented in this guide runs on **Windows 64-bit** and requires:

- A 64-bit **Java Runtime Environment, version 11 or later**
- No separate installation needed: download `stepss.jar` and run it directly; it self-extracts at startup

To compile user-defined models (CODEGEN) on Windows:

- **Visual Studio Community 2019 or later** (2022 recommended) with *Desktop development with C++*
- **Intel oneAPI 2024.0 or later** (Base Toolkit + HPC Toolkit), which provides the `ifx` Fortran compiler. The older `ifort` is deprecated and is no longer supported.

STEPSS is not Windows-only. RAMSES also builds on Linux and macOS with gfortran, and PyRAMSES installs with `pip` on Windows and Linux — see [stepss.sps-lab.org](https://stepss.sps-lab.org/).

## Limitations (free academic version)

- Maximum **1000 buses/nodes**
- Maximum **2 processor cores** for parallelisation

## Documentation

- **Published guide (PDF)**: [stepss.sps-lab.org/stepss_docs.pdf](https://stepss.sps-lab.org/stepss_docs.pdf)
- **Documentation website**: [stepss.sps-lab.org](https://stepss.sps-lab.org/) — the same content published as a website, built from the [stepss-docs](https://github.com/SPS-L/stepss-docs) repository

## License

This guide — the LaTeX sources, the figures, and the built PDF — is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license — see [LICENSE](LICENSE). Share and adapt it freely, including commercially, with appropriate credit.

**The software it documents is not.** STEPSS is distributed under the **Academic Public License**: free for non-commercial use (academic research, teaching, non-profit organisations); commercial use requires contacting the authors, and the free version is limited to 1000 buses and 2 cores. Those terms are reproduced in `legal.tex` and printed at the front of the guide.

PFC and CODEGEN are the property of Dr. Thierry Van Cutsem. RAMSES is the property of the University of Liège (Belgium); the authors hold distribution rights.

See [NOTICE](NOTICE).

## Authors

Developed and maintained by the [Sustainable Power Systems Laboratory (SPS-L)](https://sps-lab.org/) at the Cyprus University of Technology, under the direction of Dr. Petros Aristidou.

The guide is authored by:

- **Dr. Petros Aristidou** — petros.aristidou@cut.ac.cy
- **Dr. Thierry Van Cutsem** — thierry.h.van.cutsem@gmail.com
