# STEPSS User Guide

This repository contains the LaTeX source for the **STEPSS** documentation — models reference and user guide.

## What is STEPSS?

**STEPSS** (*Static and Transient Electric Power Systems Simulation*) is an open-source power system simulation tool for dynamic studies of electrical grids. It performs power flow computations and simulates the dynamic response of power systems to disturbances under the phasor approximation.

STEPSS consists of three tightly integrated modules:

| Module | Full Name | Description |
|--------|-----------|-------------|
| **PFC** | Power Flow Computation | Determines the initial operating point using the Newton-Raphson method in polar coordinates. Computes bus voltage magnitudes and phase angles, with optional transformer ratio adjustment. |
| **RAMSES** | RApid Multithreaded Simulation of Electric power Systems | Simulates the dynamic evolution of the power system. Supports Backward Euler, Trapezoidal, and BDF2 integration methods. Exploits OpenMP parallelism (up to 2 cores in the free version). |
| **CODEGEN** | CODE GENerator | Translates user-defined models from text descriptions into Fortran 2003 code for compilation and linking. Supports excitation controllers, torque controllers, injectors, and two-port components. |

## Repository Structure

The documentation is a modular LaTeX project. The main entry point is `stepss_doc.tex`, which includes the following chapters:

- `overview.tex` — Quick overview of STEPSS
- `install.tex` — Installation instructions
- `network.tex` — Network modeling (buses, lines, transformers)
- `ref_and_init.tex` — Reference frame and initialization
- `pfc-data.tex` — PFC module data format
- `files.tex`, `solvsett.tex` — File formats and solver settings
- `disturbances.tex` — Disturbance specification
- `disc_cont.tex`, `sync_thev_impload.tex` — Dynamic models
- `user_models.tex` — User-defined models (CODEGEN)
- `library_blocks.tex` — Library of modeling blocks
- `codegen/` — Individual block definitions

## Building the PDF

Compile with `pdflatex` (run twice to resolve cross-references):

```bash
pdflatex stepss_doc.tex
pdflatex stepss_doc.tex
```

Two pre-built PDFs are included, and they are kept identical:

- `stepss_doc.pdf` — the build output of `stepss_doc.tex`, and the copy published at [stepss.sps-lab.org](https://stepss.sps-lab.org).
- `userguide.pdf` — the same document under the file name the Java GUI expects. It is bundled, together with the companion notes in `models/`, into `DOC.zip` in [stepss-java-ui](https://github.com/SPS-L/stepss-java-ui) and extracted at run time.

After rebuilding, refresh both, and regenerate `DOC.zip` in `stepss-java-ui` so the GUI ships the current guide.

## Prerequisites (running STEPSS)

The Java GUI documented in this guide runs on **Windows 64-bit** and requires:

- A 64-bit **Java Runtime Environment, version 11 or later**
- No separate installation needed: download `stepss.jar` and run it directly; it self-extracts at startup

To compile user-defined models (CODEGEN) on Windows:

- **Visual Studio Community 2019 or later** (2022 recommended) with *Desktop development with C++*
- **Intel oneAPI 2024.0 or later** (Base Toolkit + HPC Toolkit), which provides the `ifx` Fortran compiler. The older `ifort` is deprecated and is no longer supported.

STEPSS is not Windows-only. RAMSES also builds on Linux and macOS with gfortran, and PyRAMSES installs with `pip` on Windows and Linux — see [stepss.sps-lab.org](https://stepss.sps-lab.org).

## Limitations (free academic version)

- Maximum **1000 buses/nodes**
- Maximum **2 processor cores** for parallelisation

## License

**This guide** — the LaTeX sources, the figures and the built PDF — is licensed
[CC BY 4.0](LICENSE). Share and adapt it freely, including commercially, with
appropriate credit.

**The software it documents is not.** STEPSS is distributed under the **Academic
Public License**: free for non-commercial use (academic research, teaching,
non-profit organisations); commercial use requires contacting the authors, and
the free version is limited to 1000 buses and 2 cores. Those terms are
reproduced in `legal.tex` and printed at the front of the guide.

PFC and CODEGEN are owned by the authors. RAMSES is owned by the University of Liège (Belgium); the authors hold distribution rights.

See [NOTICE](NOTICE).

## Authors

- **Dr. Petros Aristidou** — petros.aristidou@cut.ac.cy
- **Dr. Thierry Van Cutsem** — thierry.h.van.cutsem@gmail.com
