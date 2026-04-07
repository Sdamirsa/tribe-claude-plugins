# Releasing Guide — tribe-claude-plugins

Your personal playbook for creating new plugins and shipping updates in this monorepo. Follow it top to bottom the first time; after that it becomes muscle memory.

---

## Conventions (read once, obey always)

- **One folder per plugin**, at the repo root. Folder name = plugin name.
- **Plugin names** are kebab-case and must match `.claude-plugin/plugin.json` `"name"`.
- **Version tags** are prefixed so each plugin releases independently:
  `<plugin-name>-vX.Y.Z[-prerelease]`
- **Semver rules:**
  - `0.x.y` = unstable / pre-1.0. Use this while the workflow is still shifting.
  - `-alpha` = internal / untested. `-beta` = tested by you but not others. `-rc1` = release candidate.
  - Bump **patch** (`0.1.0 → 0.1.1`) for typo fixes, tiny skill edits.
  - Bump **minor** (`0.1.0 → 0.2.0`) for new skills, new behaviors, non-breaking changes.
  - Bump **major** (`0.x → 1.0`) when you're confident the workflow is stable; `1.x → 2.x` for breaking changes.
- **Every release** has both a git tag and a GitHub Release with the `.plugin` file attached. A tag without a Release is invisible to users.
- **`.claude-plugin/marketplace.json`** at the repo root lists every plugin. Update it every time you add a new plugin.

---

## Part A — Creating a new plugin

### 1. Scaffold the folder

```bash
cd tribe-claude-plugins
mkdir -p my-new-plugin/.claude-plugin
mkdir -p my-new-plugin/skills
```

### 2. Write `plugin.json`

`my-new-plugin/.claude-plugin/plugin.json`:

```json
{
  "name": "my-new-plugin",
  "version": "0.1.0-alpha",
  "description": "One sentence describing what this plugin does.",
  "author": { "name": "Amir (AI-CVM Bern)" },
  "keywords": ["keyword1", "keyword2"],
  "license": "MIT"
}
```

Rules:
- `name` must exactly match the folder name.
- `version` here is informational; the git tag is the source of truth.
- `keywords` help discovery in marketplaces.

### 3. Add skills

Each skill lives in `skills/<skill-name>/SKILL.md` with YAML frontmatter:

```markdown
---
name: skill-name
description: When to trigger this skill. Be specific about verbs and nouns users will say.
---

# Skill Name

Instructions for Claude go here...
```

Optional: `skills/<skill-name>/references/` folder for long reference material Claude reads only when needed (progressive disclosure).

### 4. Add a plugin-level README

`my-new-plugin/README.md` — explain what the plugin does, what skills it contains, any external dependencies, and how to use it.

### 5. Register it in the monorepo marketplace

Edit `.claude-plugin/marketplace.json` at the repo root and add an entry:

```json
{
  "name": "my-new-plugin",
  "source": "./my-new-plugin",
  "description": "Short description from plugin.json."
}
```

### 6. Update the top-level README table

Add a row to the plugin table in the root `README.md` so visitors see it.

### 7. Build the two release artifacts

Every plugin ships **two** distributable files:

1. **`<plugin-name>.plugin`** — native plugin bundle for Cowork, Claude Code, and VS Code.
2. **`plugin-as-skill-for-web.zip`** — web-compatible bundle for claude.ai Projects (since Claude Chat has no plugin support).

Build both:

```bash
cd my-new-plugin

# Artifact 1: native plugin bundle
zip -rq my-new-plugin.plugin . \
  -x "*.DS_Store" "*.git*" "plugin-as-skill-for-web.zip"

# Artifact 2: web bundle (skills + PROJECT-INSTRUCTIONS + web README)
rm -rf /tmp/web-stage && mkdir -p /tmp/web-stage
cp -r skills /tmp/web-stage/skills
cp claude-chat-project-instructions.md /tmp/web-stage/PROJECT-INSTRUCTIONS.md
cp WEB-README.md /tmp/web-stage/README.md   # write this once per plugin
(cd /tmp/web-stage && zip -rq "$OLDPWD/plugin-as-skill-for-web.zip" . -x "*.DS_Store")

cd ..
```

Keep both files checked in next to the plugin sources so users can grab them without running zip themselves. Both get attached to the same GitHub Release.

**WEB-README.md** (write once per plugin) should contain:
- What's inside the zip
- How to install in a Claude Project (upload knowledge + paste instructions)
- Limitations vs. the native plugin (no script execution, weaker auto-triggering)
- Which plugin version this bundle corresponds to

### 8. Commit

```bash
git add my-new-plugin .claude-plugin/marketplace.json README.md
git commit -m "Add my-new-plugin v0.1.0-alpha"
git push
```

### 9. Tag and release

See **Part C — Tag and release** below.

---

## Part B — Updating an existing plugin

### 1. Make your changes

Edit SKILL.md files, add new skills, fix bugs, whatever.

### 2. Bump the version in `plugin.json`

Follow semver:
- Typo fix: `0.1.0 → 0.1.1`
- New skill or new feature: `0.1.1 → 0.2.0`
- Breaking rename or workflow change (pre-1.0): bump minor.
- Graduating out of alpha: `0.2.0-alpha → 0.2.0` (drop the `-alpha`).
- Going stable: `0.9.0 → 1.0.0`.

### 3. Rebuild both release artifacts

```bash
cd <plugin-folder>

# Native plugin
rm -f <plugin-name>.plugin
zip -rq <plugin-name>.plugin . \
  -x "*.DS_Store" "*.git*" "plugin-as-skill-for-web.zip"

# Web bundle
rm -rf /tmp/web-stage && mkdir -p /tmp/web-stage
cp -r skills /tmp/web-stage/skills
cp claude-chat-project-instructions.md /tmp/web-stage/PROJECT-INSTRUCTIONS.md
cp WEB-README.md /tmp/web-stage/README.md
rm -f plugin-as-skill-for-web.zip
(cd /tmp/web-stage && zip -rq "$OLDPWD/plugin-as-skill-for-web.zip" . -x "*.DS_Store")

cd ..
```

### 4. Update the plugin's own `CHANGELOG.md` (optional but recommended)

```markdown
## v0.2.0 — 2026-04-20

### Added
- new skill: paper-translator

### Fixed
- typo in crystalit-noter frontmatter

### Changed
- clarified trigger conditions for scope-strategist
```

### 5. Commit

```bash
git add <plugin-folder>
git commit -m "<plugin-name> v0.2.0: add paper-translator skill"
git push
```

### 6. Tag and release

See **Part C** below.

---

## Part C — Tag and release (the 5-minute ritual)

Do this every time, for both new plugins and updates.

### 1. Create and push the tag

```bash
# From the repo root
git tag <plugin-name>-v<version>
git push origin <plugin-name>-v<version>
```

Examples:
```bash
git tag claude-research-junior-v0.2.0
git push origin claude-research-junior-v0.2.0
```

### 2. Create the GitHub Release

Go to https://github.com/Sdamirsa/tribe-claude-plugins/releases → **Draft a new release**.

- **Choose a tag**: pick the tag you just pushed.
- **Release title**: `<Plugin Nice Name> v<version>` (e.g., `Claude Research Junior v0.2.0`).
- **Description**: short changelog. Copy from the plugin's CHANGELOG.md if you keep one.
- **Pre-release checkbox**: ☑ tick it for any `0.x`, `-alpha`, `-beta`, or `-rc` release. Untick for stable `1.0+` releases.
- **Attach binaries**: drag **both** `<plugin-folder>/<plugin-name>.plugin` **and** `<plugin-folder>/plugin-as-skill-for-web.zip` into the assets box. **This is the critical step — without them there is nothing to download.**
- Click **Publish release**.

### 3. Verify

Open an incognito window and visit the Releases page. Confirm the `.plugin` file is listed as an asset and the download link works.

### 4. Announce (optional)

If your teammates are already using an earlier version, tell them to run:

```bash
# Claude Code users
/plugin update claude-research-junior@tribe-claude-plugins
```

Cowork users just reinstall from the repo URL.

---

## Part D — Fixing mistakes

### I tagged the wrong version

```bash
# Delete on GitHub
git push origin :refs/tags/<wrong-tag>

# Delete locally
git tag -d <wrong-tag>

# Recreate
git tag <right-tag>
git push origin <right-tag>
```

If you already created a GitHub Release from the wrong tag, delete that Release first in the UI.

### I forgot to attach the .plugin file to a Release

Go to the Release page → **Edit** → drag the file into the assets box → **Update release**. No retag needed.

### I pushed broken skills

Bump the patch version, fix, rebuild, retag, re-release. Don't try to edit history — it's easier to ship `v0.2.1` than to rewrite `v0.2.0`.

### The marketplace.json is wrong

Edit, commit, push. The next `/plugin marketplace update` on a user's machine picks it up.

---

## Part E — Quality checklist before every release

Run through this before tagging. Takes 2 minutes.

- [ ] `plugin.json` version bumped and matches intended tag.
- [ ] Every new or changed `SKILL.md` has valid YAML frontmatter (`name`, `description`).
- [ ] `description` fields are specific about trigger phrases so Claude actually invokes the skill.
- [ ] No hardcoded absolute paths from your machine.
- [ ] No secrets, API keys, or personal identifiers in any file.
- [ ] `.plugin` file rebuilt and committed.
- [ ] `plugin-as-skill-for-web.zip` rebuilt and committed.
- [ ] Plugin README updated if behavior changed.
- [ ] Monorepo `README.md` plugin table updated if it's a new plugin.
- [ ] `marketplace.json` updated if it's a new plugin.
- [ ] Local smoke test: install from the local folder in Claude Code and confirm at least one skill triggers.

```bash
# Local smoke test
claude
/plugin marketplace add ./
/plugin install <plugin-name>@tribe-claude-plugins
# then try a prompt that should trigger one of the skills
```

---

## Part F — Optional automation (do this when manual releases get annoying)

Add `.github/workflows/release.yml` to auto-build and attach `.plugin` files whenever you push a tag:

```yaml
name: Release plugin

on:
  push:
    tags:
      - '*-v*'

jobs:
  build-and-release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Parse plugin name from tag
        id: parse
        run: |
          TAG="${GITHUB_REF_NAME}"
          PLUGIN="${TAG%-v*}"
          VERSION="${TAG##*-v}"
          echo "plugin=$PLUGIN" >> $GITHUB_OUTPUT
          echo "version=$VERSION" >> $GITHUB_OUTPUT

      - name: Build .plugin file
        run: |
          cd "${{ steps.parse.outputs.plugin }}"
          zip -rq "${{ steps.parse.outputs.plugin }}.plugin" . \
            -x "*.DS_Store" "*.git*"

      - name: Create GitHub Release
        uses: softprops/action-gh-release@v2
        with:
          files: ${{ steps.parse.outputs.plugin }}/${{ steps.parse.outputs.plugin }}.plugin
          prerelease: ${{ contains(github.ref_name, '-alpha') || contains(github.ref_name, '-beta') || contains(github.ref_name, '-rc') }}
          generate_release_notes: true
```

After this is committed, your release ritual shrinks to:

```bash
git tag <plugin-name>-v<version>
git push origin <plugin-name>-v<version>
```

That's it. GitHub builds the zip, creates the Release, attaches the file, and auto-generates notes from commit messages.

---

## TL;DR cheat sheet

**New plugin:**
1. `mkdir my-plugin`, add `plugin.json`, skills, README
2. Add entry to root `marketplace.json` and `README.md` table
3. `zip -rq my-plugin.plugin .`
4. Commit, push
5. Tag `my-plugin-v0.1.0-alpha`, push tag
6. Draft Release on GitHub, attach `.plugin` file, tick pre-release, publish

**Update:**
1. Edit files, bump `plugin.json` version
2. Rebuild `.plugin` file
3. Commit, push
4. Tag `<plugin>-v<new-version>`, push tag
5. Draft Release, attach `.plugin` file, publish

**Fix a bad release:** bump patch, don't rewrite history.
