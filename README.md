# HEP-Genesis-Agent — releases

Alpha builds of **HEP-Genesis-Agent**, a desktop + CLI agent for computational
science: a chat agent that drives MCP tool servers and dispatches compute to
DOE HPC facilities (ALCF Polaris, NERSC Perlmutter) via IRI + Globus.

This repository carries **installers and metadata only** — the source lives in
a private repository during the alpha. Report issues to the maintainers
directly.

## Install (alpha)

Grab the latest build from
[Releases](https://github.com/HEP-KE/hep-genesis-releases/releases):

| Platform | File |
|---|---|
| macOS | `HEP-Genesis-Agent-<version>.dmg` |
| Windows | `HEP-Genesis-Agent-Setup-<version>.exe` |
| Linux | `HEP-Genesis-Agent-<version>.AppImage` or `.deb` |

### macOS: "HEP-Genesis-Agent is damaged and can't be opened"

The app is **not damaged** — alpha builds are unsigned, and macOS shows this
misleading dialog for any unsigned app downloaded with a browser
(right-click → Open does **not** help for this variant). After copying the
app to `/Applications`, run this once in Terminal:

```bash
xattr -dr com.apple.quarantine "/Applications/HEP-Genesis-Agent.app"
```

Then open it normally. (This clears macOS's download-quarantine flag; you
are trusting this build — that's what alpha testing is.)

### Python backend (the `.whl` file)

The desktop app drives a Python backend for MCP servers and HPC dispatch.
Download the `.whl` from the release, then install it into a Python ≥ 3.10
environment (conda or venv):

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
