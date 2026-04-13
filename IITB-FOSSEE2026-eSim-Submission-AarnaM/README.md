# IIT Bombay FOSSEE 2026
## eSim 2.5 — Ubuntu 25.04 Installation Bug Report

**Author:** Aarna Manjunath  
**Repo:** 

**eSim Version:** 2.5  
**Test OS:** Ubuntu 25.04 (Plucky Puffin)  
**Date:** 13th April 2026



## Overview

This report documents the bugs encountered when attempting to install eSim 2.5 on Ubuntu 25.04. The `install-eSim.sh` script was originally written for Ubuntu 22.04–24.04. Ubuntu 25.04 introduces breaking changes in system libraries, Python packaging rules, and deprecated tools that cause the installer to fail at multiple stages.



## Bug Summary

| # | Bug | Severity | Status |
|---|-----|----------|--------|
| 01 | Ubuntu 25.04 not in version check — script exits immediately | 🔴 Critical | ✅ Fixed |
| 02 | KiCad PPA has no Plucky (25.04) release — 404 error | 🔴 Critical | ✅ Fixed (workaround) |
| 03 | `apt-key` command removed in Ubuntu 25.04 | 🟠 High | ✅ Fixed |
| 04 | `/root/.gnupg` directory missing, GPG operations fail | 🟠 High | ✅ Fixed |
| 05 | KiCad PPA packages incompatible with Ubuntu 25.04 libraries | 🔴 Critical | ✅ Fixed (workaround) |



## Files Changed

- `install-eSim.sh` — added `25.04` case to version check
- `install-eSim-scripts/install-eSim-24.04.sh` — KiCad PPA and GPG key handling for Ubuntu 25.04



## How to Read This Report

Each bug has its own dedicated file:

- [`bug-01-ubuntu-version-check.md`](./bug-01-ubuntu-version-check.md)
- [`bug-02-kicad-ppa-no-plucky.md`](./bug-02-kicad-ppa-no-plucky.md)
- [`bug-03-apt-key-removed.md`](./bug-03-apt-key-removed.md)
- [`bug-04-gnupg-missing.md`](./bug-04-gnupg-missing.md)
- [`bug-05-kicad-library-conflicts.md`](./bug-05-kicad-library-conflicts.md)


Each file contains: error message, root cause, fix applied, impact level, and recommendations.
