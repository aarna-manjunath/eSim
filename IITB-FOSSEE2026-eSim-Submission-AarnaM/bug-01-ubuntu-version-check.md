# Bug 01: Ubuntu 25.04 Not Supported in Version Check

## Status
✅ Fixed

## Environment
- OS: Ubuntu 25.04 (Plucky Puffin)
- eSim Version: 2.5
- File Affected: `install-eSim.sh`

## Error Message
```
Unsupported Ubuntu version: 25.04
```
The script exits immediately without installing anything.

## Root Cause
The `install-eSim.sh` script uses a `case` statement to detect the Ubuntu version and select the appropriate sub-script. It only handles versions `22.04`, `23.04`, and `24.04`. When run on Ubuntu 25.04, it hits the wildcard `*` case and exits with code 1.

```bash
# Affected section in install-eSim.sh
case $VERSION_ID in
    "22.04")
        ...
        ;;
    "23.04")
        SCRIPT="$SCRIPT_DIR/install-eSim-23.04.sh"
        ;;
    "24.04")
        SCRIPT="$SCRIPT_DIR/install-eSim-24.04.sh"
        ;;
    *)
        echo "Unsupported Ubuntu version: $VERSION_ID ($FULL_VERSION)"
        exit 1   # <-- script exits here on Ubuntu 25.04
        ;;
esac
```

## Available Sub-Scripts
```
install-eSim-22.04.sh
install-eSim-23.04.sh
install-eSim-24.04.sh
```
There is no `install-eSim-25.04.sh` yet.

## Fix Applied
Added a `25.04` case that falls back to the `24.04` script as a workaround, with a clear warning message:

```bash
"24.04")
    SCRIPT="$SCRIPT_DIR/install-eSim-24.04.sh"
    ;;
"25.04")
    echo "Warning: Ubuntu 25.04 not officially supported. Attempting with 24.04 script..."
    SCRIPT="$SCRIPT_DIR/install-eSim-24.04.sh"
    ;;
*)
    echo "Unsupported Ubuntu version: $VERSION_ID ($FULL_VERSION)"
    exit 1
    ;;
```


## Permanent Fix Recommendation
A dedicated `install-eSim-25.04.sh` script should be created, adapted for Ubuntu 25.04's updated system libraries, Python version, and package availability.
