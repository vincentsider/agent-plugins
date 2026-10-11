# Prodigo agent plugins

Plugins that connect coding agents to the Prodigo service for Pega delivery. One package,
every host: Claude Code, Codex and ChatGPT, GitHub Copilot, Cursor, OpenClaw, and the
claude.ai connectors.

| Plugin | What it does |
|---|---|
| [`prodigo`](plugins/prodigo) | Prodigo for Pega: grounded Jira backlogs from Blueprints, story quality, Pega Infinity technical designs, builds into an isolated Pega branch, documentation of existing applications, an independent judge, migration planning. |

Install instructions, skills and prerequisites: [plugins/prodigo/README.md](plugins/prodigo/README.md).
Site: https://prodigo.ai/plugin. Privacy: https://prodigo.ai/privacy. Terms: https://prodigo.ai/terms.

This repository is a marketplace for Claude Code (`.claude-plugin/marketplace.json`),
Codex (`.agents/plugins/marketplace.json`) and GitHub Copilot CLI (`.github/plugin/marketplace.json`).

Apache-2.0 for the package. The Prodigo service is proprietary. Pegasystems, Pega, and the
Pega logo are trademarks or registered trademarks of Pegasystems Inc. and/or its affiliates;
Prodigo is not affiliated with or endorsed by Pegasystems.
