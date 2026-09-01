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
| macOS | `HEP-Genesis-Agent-<version>.dmg` (unsigned during alpha: right-click → Open the first time, or `xattr -dr com.apple.quarantine "/Applications/HEP-Genesis-Agent.app"`) |
| Windows | `HEP-Genesis-Agent-Setup-<version>.exe` |
| Linux | `HEP-Genesis-Agent-<version>.AppImage` or `.deb` |

The HPC backend is a Python package (wheel attached to each release):

```bash
pip install hep_genesis-<version>-py3-none-any.whl
```

Point the app's HPC panel at the Python environment you installed it into
(the app also auto-detects suitable interpreters on first launch).

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
