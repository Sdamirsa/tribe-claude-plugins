# tribe-claude-plugins

A personal monorepo of Claude plugins built by Amir (AI-CVM Bern) for research, writing, and scientific workflows. Each plugin lives in its own folder and is released independently with a prefixed version tag.

> ⚠️ **Early releases.** Most plugins here are alpha-stage previews. They encode real workflows that work for me, but they have not been battle-tested across users or projects. Feedback and issues are very welcome.

## Plugins in this repo

| Plugin | Status | What it does |
|---|---|---|
| [`claude-research-junior`](./claude-research-junior) | v0.1.0-alpha | Complete AI-assisted scientific paper writing system. 12 skills covering literature search, the CrystaLit literature-crystallization pipeline, scope strategy, paper architecture, figure planning, polishing, and panel discussions. |

More plugins will be added over time. Each gets its own folder and its own release tag.

## Installation

Pick your Claude client, then your operating system.

<details>
<summary><b>🖥️ Cowork (Claude desktop app)</b> — native plugin support</summary>

<details>
<summary>macOS</summary>

1. Open the Cowork app.
2. Start a new chat and type:
   > Install the plugin from `https://github.com/Sdamirsa/tribe-claude-plugins`
3. Approve the install prompt. Done.

**Alternative (offline):** Download `claude-research-junior.plugin` from the [Releases page](https://github.com/Sdamirsa/tribe-claude-plugins/releases), then drag it into a Cowork chat window.

</details>

<details>
<summary>Windows</summary>

1. Open the Cowork app.
2. Start a new chat and type:
   > Install the plugin from `https://github.com/Sdamirsa/tribe-claude-plugins`
3. Approve the install prompt. Done.

**Alternative (offline):** Download `claude-research-junior.plugin` from the [Releases page](https://github.com/Sdamirsa/tribe-claude-plugins/releases), then drag it into a Cowork chat window.

</details>

<details>
<summary>Linux</summary>

Cowork desktop support on Linux is limited. Use Claude Code (terminal) instead — see the next section.

</details>

</details>

<details>
<summary><b>⌨️ Claude Code (terminal CLI)</b> — native plugin support via marketplace</summary>

<details>
<summary>macOS</summary>

```bash
# Install Claude Code if you haven't already
npm install -g @anthropic-ai/claude-code

# Launch it and add the marketplace
claude
```

Then inside the Claude Code REPL:
```
/plugin marketplace add Sdamirsa/tribe-claude-plugins
/plugin install claude-research-junior@tribe-claude-plugins
```

Plugin files land in `~/.claude/plugins/cache/`. No manual copying.

</details>

<details>
<summary>Linux</summary>

```bash
npm install -g @anthropic-ai/claude-code
claude
```

Then inside the REPL:
```
/plugin marketplace add Sdamirsa/tribe-claude-plugins
/plugin install claude-research-junior@tribe-claude-plugins
```

Plugin files land in `~/.claude/plugins/cache/`.

</details>

<details>
<summary>Windows (WSL recommended)</summary>

Claude Code runs best inside WSL2 on Windows.

```bash
# Inside your WSL terminal
npm install -g @anthropic-ai/claude-code
claude
```

Then inside the REPL:
```
/plugin marketplace add Sdamirsa/tribe-claude-plugins
/plugin install claude-research-junior@tribe-claude-plugins
```

</details>

<details>
<summary>Project-scoped auto-install (any OS)</summary>

To make the plugin auto-install for everyone opening a specific research repo, add `.claude/settings.json` to that repo:

```json
{
  "marketplaces": { "tribe": "Sdamirsa/tribe-claude-plugins" },
  "plugins": { "claude-research-junior@tribe": "latest" }
}
```

When a teammate opens the repo in Claude Code and trusts the folder, they're prompted to install automatically.

</details>

</details>

<details>
<summary><b>🧩 VS Code Claude extension</b> — uses the same plugin system as Claude Code</summary>

<details>
<summary>macOS / Linux / Windows</summary>

The VS Code Claude extension shares plugin storage with Claude Code CLI. Install once via the CLI and it becomes available in VS Code automatically.

1. Install Claude Code CLI (see the Claude Code section above for your OS).
2. Run the marketplace commands once:
   ```
   /plugin marketplace add Sdamirsa/tribe-claude-plugins
   /plugin install claude-research-junior@tribe-claude-plugins
   ```
3. Open VS Code. The skills are now available in the Claude panel.

If you prefer a per-project install, add `.claude/settings.json` (see the Claude Code → Project-scoped section above) to your workspace root.

</details>

</details>

<details>
<summary><b>🌐 Claude Chat (web app)</b> — no native plugin support, use the "plugin-as-skill-for-web" bundle</summary>

Claude Chat (claude.ai) does not install plugins, but each plugin ships a `plugin-as-skill-for-web.zip` bundle that repackages the same skills as plain markdown files you can upload to a Claude Project.

1. Download `plugin-as-skill-for-web.zip` from the [Releases page](https://github.com/Sdamirsa/tribe-claude-plugins/releases) and unzip it.
2. Go to [claude.ai](https://claude.ai) → **Projects** → **New project**. Name it whatever you like (e.g., *Research Junior*).
3. Open `PROJECT-INSTRUCTIONS.md` from the unzipped bundle. Copy its full contents and paste them into the project's **Custom Instructions** field.
4. Drag the entire `skills/` folder into the project's **Knowledge** area. Claude Chat will flatten it and index every `SKILL.md`.
5. Start a new chat inside the project. If a skill doesn't auto-trigger, name it explicitly (e.g., *"use the scope-strategist skill to…"*).

Works on any OS since Claude Chat is browser-based. Limitations: no script execution, less precise auto-triggering, and no progressive disclosure of `references/` files.

</details>

## Repository layout

```
tribe-claude-plugins/
├── README.md                          ← you are here
├── .claude-plugin/
│   └── marketplace.json               ← lists every plugin in this repo
├── claude-research-junior/
│   ├── .claude-plugin/plugin.json
│   ├── skills/                        ← 12 skills
│   ├── claude-research-junior.plugin  ← built artifact, attached to releases
│   ├── init-paper.sh                  ← standalone project setup script
│   ├── claude-chat-project-instructions.md
│   └── README.md
└── (future plugins go here)
```

## Release convention

Each plugin is released independently with a prefixed semver tag:

```
<plugin-name>-vX.Y.Z[-prerelease]
```

Examples:
- `claude-research-junior-v0.1.0-alpha`
- `claude-research-junior-v0.2.0`
- `some-future-plugin-v1.0.0`

Every GitHub Release attaches the corresponding `.plugin` file as a downloadable asset. Pre-1.0 and `-alpha` / `-beta` releases are marked as pre-releases on GitHub.

## Building a plugin locally

```bash
cd <plugin-folder>
zip -rq <plugin-name>.plugin . -x "*.DS_Store" "*.git*"
```

The resulting `.plugin` file is a standard zip that both Cowork and Claude Code understand.

## Contributing / feedback

These plugins encode opinionated workflows. If something doesn't fit yours, fork the folder and adapt. Issues and PRs welcome — please scope each PR to a single plugin.

## License

MIT, unless a specific plugin folder states otherwise.

## Author

Amir — AI-CVM Bern
