# HEP-Genesis-Agent — releases

Alpha builds of **HEP-Genesis-Agent**, a desktop + CLI agentic client or harness for HEP: a chat agent that drives MCP tool servers and dispatches compute to
DOE HPC facilities (ALCF Polaris, NERSC Perlmutter) via IRI + Globus.

This repository carries **installers and metadata only** — the source lives in
a private repository during the alpha. Report issues to the maintainers
directly.

## Install (alpha)

Grab the latest build from
[Releases](https://github.com/HEP-KE/hep-genesis-releases/releases):

| Platform | File |
|---|---|
| macOS (Apple Silicon) | `HEP-Genesis-Agent-<version>-arm64.dmg` |
| Windows (x64) | `HEP-Genesis-Agent-Setup-<version>-x64.exe` |
| Linux (x64) | `HEP-Genesis-Agent-<version>-x86_64.AppImage` or `HEP-Genesis-Agent-<version>-amd64.deb` |

**Most testers only need the app from the dmg file** Chat, research runs, reports, and
remote (hosted) MCP tool servers — including their OAuth sign-in — work with
no Python installed. The `.whl`: local
science MCP servers and dispatching compute to ALCF/NERSC, which also
requires a facility account with an allocation and
[Globus Connect Personal](https://www.globus.org/globus-connect-personal)
running. Without it the app simply shows explanatory messages in the HPC
panel; everything else works.

### macOS warning may show: "HEP-Genesis-Agent is damaged and can't be opened"

The app is **not damaged** — alpha builds are unsigned, and macOS shows this
misleading dialog for any unsigned app downloaded with a browser
(right-click → Open does **not** help for this variant). After copying the
app to `/Applications`, run this once in Terminal:

```bash
xattr -dr com.apple.quarantine "/Applications/HEP-Genesis-Agent.app"
```

Then open it normally. (This clears macOS's download-quarantine flag; you
are trusting this build — that's what alpha testing is.)

### Windows: "Windows protected your PC" (SmartScreen)

Same cause — the installer is unsigned during the alpha. Click
**More info → Run anyway**. The one-click installer puts the app under
`%LOCALAPPDATA%\Programs\HEP-Genesis-Agent` and adds a Start-menu entry.

### Linux

- **AppImage**: `chmod +x HEP-Genesis-Agent-*.AppImage && ./HEP-Genesis-Agent-*.AppImage`
  (needs FUSE 2: `sudo apt install libfuse2` on Ubuntu 22.04+).
- **.deb**: `sudo apt install ./HEP-Genesis-Agent-*-amd64.deb` — installs to
  `/opt/HEP-Genesis-Agent` with a desktop entry.

### Python backend (the `.whl` file) 

The desktop app drives a Python backend for local MCP servers and HPC
dispatch. Download the `.whl` from the release, then install it into a
Python ≥ 3.10 environment (conda or venv):

```bash
conda create -n hep-genesis python=3.12 -y
conda activate hep-genesis
pip install 'hep_genesis-<version>-py3-none-any.whl[all]'
```

(The `[all]` extra pulls the agent + service + HPC dependencies. Use the
real filename you downloaded.)

Launch the app — it auto-detects conda environments that contain the
backend on first run; if it doesn't find yours, point the HPC panel's
"Pick python" at that environment's `python`.

## First-run checklist

1. Pick an LLM endpoint in Settings (presets included; paste your API key).
2. Connect the seeded MCP servers you need (Servers panel).
3. For HPC dispatch: sign in to your facility in the HPC panel and have
   [Globus Connect Personal](https://www.globus.org/globus-connect-personal)
   running.

## `mcp-client-metadata.json`

This repo also publishes the app's OAuth **client ID metadata document**
(CIMD) at:

```
https://hep-ke.github.io/hep-genesis-releases/mcp-client-metadata.json
```

MCP server operators who gate OAuth clients by allowlist should allowlist
that exact URL — it identifies this app (public client, PKCE, loopback
redirect on 127.0.0.1:52814).
