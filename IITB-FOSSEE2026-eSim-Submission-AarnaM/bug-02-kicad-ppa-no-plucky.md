# Bug 02: KiCad PPA Has No Release for Ubuntu 25.04 (Plucky)

## Status
✅ Fixed (via workaround — using noble PPA entry)

## Environment
- OS: Ubuntu 25.04 (Plucky Puffin)
- eSim Version: 2.5
- File Affected: `install-eSim-scripts/install-eSim-24.04.sh`

## Error Message
```
Err: https://ppa.launchpadcontent.net/kicad/kicad-8.0-releases/ubuntu plucky Release
  404  Not Found [IP: ...]
E: The repository '...ubuntu plucky Release' does not have a Release file.
```

## Root Cause
The script adds the KiCad PPA using `add-apt-repository`, which automatically uses the current Ubuntu codename (`plucky` for 25.04). The KiCad 8.0 PPA on Launchpad only has builds up to Ubuntu 24.04 (Noble) — there is no `plucky` release available yet. This causes a 404 and blocks KiCad installation entirely.

```bash
# Affected section in install-eSim-24.04.sh (around line 147)
sudo add-apt-repository -y "ppa:$kicadppa"
# This resolves to: .../ubuntu plucky main  <-- doesn't exist
```

## Fix Applied
Instead of using `add-apt-repository` (which auto-detects the codename), manually add a PPA source file that forces the `noble` suite, which is compatible with Ubuntu 25.04:

```bash
# In install-eSim-24.04.sh, replace the PPA addition block with:
if [[ "$ubuntu_version" == "25.04" ]]; then
    # BUG FIX: KiCad PPA has no Plucky release, force noble suite as workaround
    echo "deb [signed-by=/usr/share/keyrings/kicad-archive-keyring.gpg] \
https://ppa.launchpadcontent.net/kicad/kicad-8.0-releases/ubuntu noble main" \
    | sudo tee /etc/apt/sources.list.d/kicad-esim.list
else
    sudo add-apt-repository -y "ppa:$kicadppa"
fi
```

Also added the `elif` for 25.04 in the version detection block:

```bash
elif [[ "$ubuntu_version" == "25.04" ]]; then
    echo "Ubuntu 25.04 detected. Using KiCad 8.0 PPA with noble suite as workaround."
    kicadppa="kicad/kicad-8.0-releases"
```

## Notes
- The `kicad/kicad-6.0-releases` PPA (used for older Ubuntu versions) also has no Plucky release — this applies to both PPA variants.
- As of the time of testing, the KiCad team had not yet published a Plucky-native build.
