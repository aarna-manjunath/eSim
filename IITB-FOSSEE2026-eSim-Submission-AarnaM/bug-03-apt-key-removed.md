# Bug 03: `apt-key` Command Removed in Ubuntu 25.04

## Environment
- OS: Ubuntu 25.04 (Plucky Puffin)
- eSim Version: 2.5
- File Affected: `install-eSim-scripts/install-eSim-24.04.sh`

## Error Message
```
sudo: apt-key: command not found
```

## Root Cause
The `apt-key` command was deprecated in Ubuntu 22.04 and has been fully removed in Ubuntu 25.04. The eSim install script (and the workaround attempted during Bug #02 fixing) relied on `apt-key adv` to import GPG keys for the KiCad PPA. On Ubuntu 25.04, this command no longer exists, causing the key import to silently fail or error out.

```bash
# Old method (broken on Ubuntu 25.04):
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 245D5502FAD7A805
# Output: sudo: apt-key: command not found
```

