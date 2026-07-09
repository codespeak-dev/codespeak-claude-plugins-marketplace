# Spindle — Claude Code requirements plugin

This directory is the plugin's source tree. CI publishes it on every build to
the public
[codespeak-dev/codespeak-claude-plugins-marketplace](https://github.com/codespeak-dev/codespeak-claude-plugins-marketplace)
repo (branch per release channel, build version stamped into `plugin.json`; see
`bin/publish-cc-plugin.ts` and docs/adr/0012), and the repo-root
`.claude-plugin/marketplace.json` points here so a local checkout works as a
marketplace for plugin development (`bin/spindle`). The plugin is intentionally
thin: the manifests below invoke the **`spindle-client` CLI, which must be
installed separately on your `PATH`** — the plugin does not carry a runtime.

## Install

1. Install the `spindle-client` CLI via npm (the package lives in the CodeSpeak
   GitHub Packages registry — see "Install via npm" in the
   [repo README](../../../README.md) for the one-time auth setup):
   ```sh
   npm install -g @codespeak-dev/spindle-client@dev
   ```
   Alternatively, download the self-contained binary from the release and put it
   in a directory on your `PATH` (e.g. `~/.local/bin` if your shell has it on
   `PATH` — macOS does not by default):
   ```sh
   curl -fLO https://github.com/codespeak-dev/spindle/releases/download/dev/spindle-client-<target>
   chmod +x spindle-client-<target> && mv spindle-client-<target> <dir-on-PATH>/spindle-client
   ```
   Pick `<target>` for your platform (e.g. `aarch64-apple-darwin`,
   `x86_64-unknown-linux-gnu`, `x86_64-pc-windows-msvc.exe`). Verify with
   `sha256sum --check` / `gh attestation verify` against the release.
2. Install the plugin into a project. Either let the CLI wire it up —
   ```sh
   spindle-client track <path-to-repo> [--channel dev|release]
   ```
   which runs `claude plugin marketplace add` + `claude plugin install` at
   project scope, recording both in the repo's `.claude/settings.json` — or do
   it by hand in Claude Code:
   ```sh
   /plugin marketplace add codespeak-dev/codespeak-claude-plugins-marketplace
   /plugin install spindle@codespeak
   ```
   (dev channel: append `@dev` to the marketplace source and
   `/plugin install spindle@codespeak-dev`.)

If `spindle-client` is not on `PATH`, the hooks degrade gracefully: at session
start you get a notice with install instructions, and the per-turn hooks stay
silent (no errors). The MCP server (`spindle-client cc mcp`) is not guarded, so
its tools report as not found until you install the CLI (step 1).

## What's here

- `.claude-plugin/plugin.json` — manifest; registers the `requirements` MCP
  server as `spindle-client cc mcp`.
- `hooks/hooks.json` — `SessionStart`, `UserPromptSubmit`, and `Stop` hooks, run
  as `spindle-client cc hook <event>`.
- `skills/` — bundled skills (e.g. `review-requirements`, which audits the
  tracked requirements against the extraction guidelines and fixes them).

The CLI's source and build live in `apps/client/` (the `cc` subcommand) and are
**not** shipped here.
