# Bug 04: KiCad PPA Packages Incompatible with Ubuntu 25.04 Libraries

## Environment
- OS: Ubuntu 25.04 (Plucky Puffin)
- eSim Version: 2.5
- KiCad PPA version attempted: 8.0.9 (built for Ubuntu 24.04 / noble)

## Error Message
```
The following packages have unresolvable dependencies:
  kicad: Depends: libgit2-1.7 but it is not installable
         Depends: libocct-7.6 but it is not installable
         Depends: libpython3.12 but it is not installable
E: Unable to correct problems, you have held broken packages.
```

## Root Cause
Even after successfully adding the KiCad 8.0 PPA with the correct GPG key (Bugs #02–#03), the PPA package itself (`kicad 8.0.9`) was compiled against Ubuntu 24.04's system libraries. Ubuntu 25.04 ships with newer versions of those libraries that are not backward-compatible:

| Library | Required by PPA | Available on Ubuntu 25.04 |
|---|---|---|
| `libgit2` | 1.7 | 1.9 |
| `libocct` | 7.6 | 7.8 |
| `libpython3` | 3.12 | 3.13 |

Because the PPA binary was not recompiled for Ubuntu 25.04, `apt` cannot satisfy its dependencies and refuses to install it.

