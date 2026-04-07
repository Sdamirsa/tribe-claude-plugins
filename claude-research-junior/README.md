# Dream Claude Research

A complete AI-assisted scientific paper writing system, from literature discovery to polished manuscript.

Built by Amir (AI-CVM, Bern) to give research teams a reproducible, transparent, and collaborative workflow for producing high-quality scientific papers using Claude, Zotero, Pandoc, VS Code, and Git.

---

## How It Works

This system encodes nine phases of scientific paper writing into reusable Claude skills. Each skill knows its role, its inputs and outputs, and when to pause for your confirmation. You move through phases sequentially, but you can invoke any skill independently when you need it.

```
Phase 1          Phase 2              Phase 3            Phase 4
Foundation    →   CrystaLit         →  Scope before    →  Harden
Scout             (5 sub-agents)       Scoop              Skeleton

Phase 5          Phase 6              Phase 7            Phase 8
Figure        →   Think & Write     →  Cite with       →  Comment,
Strategist        (scientific-         Markdown           Polish,
                   writing skill)     [@citekey]          Repeat
```

A standalone **Panel Discussion** skill can be called at any phase when you need multi-expert deliberation (naming decisions, methodology debates, framing choices).

### The Golden Rule

**Section 9: DOUBLE CHECK EVERYTHING. YOU ARE THE ONLY RESPONSIBLE PERSON FOR THE OUTPUT.**

Claude is a powerful collaborator, but you are the scientist. Every claim, every citation, every figure needs your eyes and judgment. The skills are designed with human-in-the-loop checkpoints precisely because AI augments your expertise, it does not replace it.

---

## Prerequisites

### Must Have (all free)

| Tool | Purpose | Install |
|------|---------|---------|
| **Computer** | Mac, PC, or Linux | You probably have one |
| **Zotero** | Reference management, open-source, many extensions | [zotero.org](https://www.zotero.org/) |
| **Better BibTeX** | Stable citation keys for Zotero | [retorque.re/zotero-better-bibtex](https://retorque.re/zotero-better-bibtex/) |
| **Pandoc** | Export markdown to docx, LaTeX, ODT, Google Docs | [pandoc.org](https://pandoc.org/installing.html) |
| **VS Code** | Editor with terminal, extensions, Git integration | [code.visualstudio.com](https://code.visualstudio.com/) |
| **Git + GitHub** | Version control, track changes, team collaboration | [git-scm.com](https://git-scm.com/) |

### VS Code Extensions (free)

| Extension | Purpose |
|-----------|---------|
| **Markdown Preview** (built-in) | View markdown while editing |
| **Citation Picker for Zotero** | Insert `[@citekey]` directly from Zotero |
| **vscode-pandoc** | Run Pandoc export from within VS Code |
| **Excalidraw** | Draw diagrams, flowcharts, and figures (highly recommended) |

### Recommended (paid)

| Tool | Purpose | Why it's worth it |
|------|---------|-------------------|
| **Claude** (Anthropic) | AI assistant powering this entire system | $20/month minimum works well. $100/month for heavy use. Honest, aligned with ethics, and the best reasoning available. |
| **Litmap** | Paper discovery and citation mapping | Finds the pool of related articles faster than any AI tool. Syncs with Zotero automatically. |

> **A note from Amir:** I hope Anthropic sees this and considers financing free hands-on workshops for developing countries to close the gap. Many talented junior researchers working with me cannot afford $20/month, yet it could make them better scientists helping the world.

---

## Quick Start

### Option A: Claude Cowork / Claude Code (recommended)

Use the init script to create a fully configured paper project in one command:

```bash
cd dream-Claude-Research
./init-paper.sh my-cardiac-ct-paper
```

This creates the project directory, copies all 12 skills into `.claude/skills/`, sets up `manuscript.md`, VS Code settings, and initializes Git. Then open Claude Cowork, select the new project folder, and all skills are auto-detected.

### Option B: Claude Chat (web / mobile)

For teams using Claude Chat on claude.ai:

1. Go to **Claude Chat → Projects → New Project**
2. Open `claude-chat-project-instructions.md` from this repo
3. Paste its contents into the Project's **Custom Instructions**
4. Any conversation in that project now has access to all phases
5. Say "Run Foundation Scout" or "Run CrystaLit" to activate a phase

> **Tip:** If the full instructions exceed the project instruction limit, split by workflow stage: create one project for "Literature Review" (Foundation Scout + CrystaLit phases) and another for "Writing" (Scope, Architect, Figure, Writing, Polishing phases).

### Option C: Manual setup

If you prefer to set things up yourself:

```bash
mkdir my-paper && cd my-paper
git init
mkdir -p papers figures notes data styles .claude/skills
cp -r /path/to/dream-Claude-Research/skills/* .claude/skills/
```

---

## Installation Details

### Step 1: Install prerequisites

Install Zotero, Better BibTeX, Pandoc, VS Code, and Git using the links above. On Mac, you can use Homebrew for Pandoc and Git:

```bash
brew install pandoc git
```

On Windows, use the installers from the official sites. On Linux:

```bash
sudo apt install pandoc git
```

### Step 2: Configure Zotero

1. Install Better BibTeX in Zotero (Tools → Add-ons → Install from file)
2. Set citation key format in Better BibTeX preferences: `auth.lower + '_' + shorttitle(3,3).lower + '_' + year`
3. Set up auto-export: right-click your collection → Export Collection → Better CSL JSON → check "Keep updated"
4. Install the Zotero Connector browser extension for Chrome/Firefox

### Step 3: Configure VS Code

Install the extensions listed above. For Pandoc export, add to your VS Code settings:

```json
{
  "pandoc.docxOptString": "--citeproc --bibliography=references.json --csl=styles/your-journal.csl"
}
```

---

## The Manuscript Structure

Everything lives in a single `manuscript.md` file:

```markdown
---
title: "Your Paper Title"
author:
  - name: First Author
    affiliation: University of Bern
    email: first@example.com
  - name: Second Author
    affiliation: Partner Institution
bibliography: references.json
csl: styles/your-journal.csl
---

# Abstract

Your abstract here...

# Introduction

...

# Methods

...

# Results

...

<!-- Figures and tables go right after their first mention -->

![Figure 1. Caption here.](figures/fig1.png){width=80%}

| Column A | Column B |
|----------|----------|
| Data     | Data     |

Table: Table 1. Caption here.

# Discussion

...

## Limitations

...

# Conclusion

...

# Statements

## Conflicts of Interest
...

## Acknowledgments
...

## Data Availability
...

# References

<!-- Auto-generated by Pandoc from [@citekeys] -->

---

# Supplementary Materials

<!-- Same title page, then supplementary content -->

## Supplementary Figures

## Supplementary Tables

## Supplementary Notes
```

---

## Zotero Collection Structure

Organize your Zotero collection with these subcollections:

```
My Paper Collection/
├── Original            # All relevant original research articles
├── Reviews             # Systematic reviews, narratives, perspectives (1-2 per relevant field)
├── Role Models         # 2-3 papers that guide your paper's tone, figures, design
├── Logic               # Papers with logical statements you need to reference
├── Interesting         # Interesting lenses on the problem
└── Maybe               # Articles you might use (not yet decided)
```

Use Zotero tags to label papers: `solid`, `role-model`, `full-text-not-found`, and any domain-specific categories.

---

## Workflow: Phase by Phase

### Phase 1: Foundation Scout

**Skill:** `foundation-scout`

Find papers manually or with Litmap. Do not rely on AI for paper discovery (as of March 2026, no AI tool reliably distinguishes original results from review claims or consistently finds full-text PDFs). Import everything into Zotero with PDFs attached. Find 1-2 review articles per relevant field to build your mental map, then find original articles. Select 2-3 role model papers that will guide your paper's design.

**HITL checkpoint:** You review your Zotero collection and confirm the foundation is complete.

### Phase 2: CrystaLit

**Skill:** `crystalit` (orchestrator) with five sub-agents

Turn your collected papers into structured knowledge:

1. **crystalit-noter**: Each PDF → structured markdown note (Data Extract, Gaps, Applications)
2. **crystalit-ontologist**: All notes → YAML ontology (Themes → Subthemes → Groups → Concepts)
3. **crystalit-labeler**: Each paper + ontology → JSON labels
4. **crystalit-vizmaker**: All labels → publication figures + interactive dashboard
5. **crystalit-writer**: Everything → literature review report

Each step has a HITL checkpoint. The orchestrator pauses between phases for your review.

### Phase 3: Scope before Scoop

**Skill:** `scope-strategist`

Define your strategy before (or after) experiments. Be ruthlessly honest about your contributions, implications, and limitations. Output: a strategy document with your main innovation, clinical implications, methodological contributions, known limitations, and framing decisions.

**HITL checkpoint:** You and your team attack the strategy. Is this the real contribution? Are we being honest about limitations?

### Phase 4: Harden Skeleton

**Skill:** `paper-architect`

Turn your strategy into a paper outline: headings, subheadings, and bullet points describing what each section should say. Blend your strategy with role model paper structures and your study's specifics. The skeleton sits on the foundation (Phase 1-2) but is unique to your paper.

**HITL checkpoint:** Review the skeleton with your team. Is the narrative arc convincing?

### Phase 5: Figure Strategist

**Skill:** `figure-strategist`

Plan every figure and table with explicit intentions: what insight does it transfer, what is its role in the narrative, and where does it appear in the manuscript. Images are the best way to transfer large amounts of insight. Tables, used correctly, give readers a grasp of information in one effort.

**HITL checkpoint:** Review the figure plan. Does every figure earn its place?

### Phase 6: Think and Write

**Skill:** `scientific-writing` (bundled in this plugin)

Go through each heading. Prepare what you want to say and your narrative for saying it. It is a story; make it a good one. The scientific-writing skill enforces paragraph architecture, active voice, and forbidden elements (no dashes, no sentence-breaking colons, no bullet points in prose).

### Phase 7: Cite with Markdown

**Approach:** Plain Markdown citations with your own bibliography.

Insert citations using the Pandoc-compatible syntax `[@citekey]` directly in your Markdown manuscript. Maintain a bibliography file (BibTeX, CSL JSON, or whatever format you prefer) in the project folder and cross-reference citation keys manually or with the VS Code Citation Picker extension. Export with Pandoc for the final formatted output. No external citation skill required.

### Phase 8: Comment, Polish, Repeat

**Skill:** `paper-polisher`

Review the manuscript at least 4 times, with at least 2 people per iteration. Check tone unity across sections, fact-check claims, verify citation accuracy, and ensure figures match the text. The polisher skill provides a structured checklist for each review round.

### Phase 9: Panel Discussion (anytime)

**Skill:** `panel-discussion` (standalone, reusable)

When you face a decision that benefits from diverse expert perspectives (model naming, methodology choice, framing debate), invoke this skill. It simulates multi-round deliberation with configurable panelists, independent voting, and convergence tracking.

---

## Repository Structure

```
dream-Claude-Research/
│
├── skills/                         All reusable skill definitions
│   ├── foundation-scout/               Phase 1: Paper discovery guidance
│   ├── crystalit/                      Phase 2: Literature crystallization (orchestrator)
│   ├── crystalit-noter/                    → PDF to markdown notes
│   ├── crystalit-ontologist/               → Notes to YAML ontology
│   ├── crystalit-labeler/                  → Papers + ontology to JSON labels
│   ├── crystalit-vizmaker/                 → Labels to figures + dashboard
│   ├── crystalit-writer/                   → Everything to literature review report
│   ├── scope-strategist/               Phase 3: Strategic planning
│   ├── paper-architect/                Phase 4: Paper skeleton
│   ├── figure-strategist/              Phase 5: Figure and table planning
│   ├── paper-polisher/                 Phase 8: Review and polish cycles
│   ├── panel-discussion/               Anytime: Multi-expert deliberation
│   └── scientific-writing/             Phase 6: Prose style enforcement
│
├── init-paper.sh                   One-command project setup script
├── claude-chat-project-instructions.md   All skills compiled for Claude Chat Projects
└── README.md                       This file
```

**External dependencies:** None. The `scientific-writing` skill is bundled with this plugin, and citations use plain Markdown `[@citekey]` syntax with your own bibliography file.

> For Claude Chat users, all skills including `scientific-writing` are compiled into `claude-chat-project-instructions.md`.

---

## Claude Tips

When prompting Claude, provide four things:

1. **Your context and intention** — Who you are, what stage you are at, what you need
2. **Your dream role for Claude** — e.g., "Act as a Nobel Laureate mentor," "Be a Nature editor"
3. **Your current task and stage** — What specifically you want done right now
4. **The desired output format** — Bullet points, prose paragraphs, a markdown file, a YAML structure

You can also use the **Panel Discussion** skill to have Claude simulate a deliberation among experts. It works remarkably well for deep thinking tasks where you need multiple perspectives before converging on a decision.

---

## For Team Members

### Using Claude Cowork (Desktop)

1. Clone this repository
2. Install all prerequisites (Steps 1-2 above)
3. Run `./init-paper.sh my-paper-name` to create your project
4. Open Claude Cowork and select the new project folder
5. All 12 skills appear automatically — say "Run Foundation Scout" to begin
6. Follow the phases in order for your first paper; after that, invoke skills as needed
7. Always push your changes to Git after each work session
8. Use pull requests for major manuscript changes so the team can review

### Using Claude Chat (Web/Mobile)

1. Open claude.ai and go to Projects
2. Create a new project for your paper
3. Paste the contents of `claude-chat-project-instructions.md` into Custom Instructions
4. Upload your PDFs and manuscript as needed in conversations
5. Say "Run [Phase Name]" to activate any phase (e.g., "Run CrystaLit" or "Run Paper Architect")

### Using Claude Code (Terminal)

1. Clone this repo and run `./init-paper.sh my-paper-name`
2. `cd my-paper-name`
3. Run `claude` — all skills in `.claude/skills/` are auto-detected
4. Use natural language: "Run the scope strategist for my paper"

---

*Built with care by Amir at AI-CVM Bern. Powered by Claude.*
