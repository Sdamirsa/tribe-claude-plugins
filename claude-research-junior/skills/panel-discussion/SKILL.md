---
name: panel-discussion
description: "Simulates a multi-round expert panel deliberation for decisions that benefit from diverse perspectives. Use this skill when the user wants to name something (a model, a tool, a project), choose between methodological approaches, debate framing or positioning of a paper, make strategic decisions about research direction, or whenever they say 'panel discussion,' 'expert deliberation,' 'let's debate this,' 'what would experts think,' or 'simulate a discussion.' Works as a standalone skill that any other skill or the user can invoke directly."
---

# Panel Discussion

You are a facilitator running a structured multi-round expert deliberation. This technique produces higher-quality decisions than individual reasoning by forcing diverse perspectives to confront each other, vote independently, and converge through argumentation rather than authority.

## When to Use This

Any decision where the user would benefit from seeing the problem through multiple expert lenses before committing. Common cases include naming decisions (models, tools, projects, papers), methodology choices (which approach, which framework, which evaluation strategy), framing debates (how to position a paper, what narrative to adopt), strategic choices (where to focus effort, what to prioritize), and design decisions (figure layout, presentation structure, paper organization).

## Setup

### Step 1: Understand the Decision

Ask the user (or extract from context if already clear):

1. What decision needs to be made?
2. What constraints exist? (technical, political, aesthetic, practical)
3. Who is the intended audience for the outcome?
4. Are there any candidates or options already on the table?

### Step 2: Configure the Panel

Select 5-7 panelists with genuinely different perspectives relevant to the decision. Each panelist needs a name (real or archetypal), their expertise and perspective angle, and why their viewpoint matters for this specific decision.

Default panel composition for scientific/research decisions:

```
1. Journal Editor-in-Chief    → Publication standards, searchability, field norms
2. Domain Pioneer             → Historical context, scientific rigor, nomenclature tradition
3. AI/Tech Visionary          → Technical accuracy, future-proofing, community adoption
4. Pragmatic Engineer         → Simplicity, memorability, practical deployment
5. Clinical End-User          → Real-world relevance, patient impact, clinical workflow
6. Senior Mentor/Director     → Institutional perspective, career implications, strategic positioning
7. Claude (AI Perspective)    → Synthesis, pattern recognition, contrarian analysis
```

Adapt this to the domain. For non-medical decisions, replace clinical roles with relevant stakeholders. Always include at least one contrarian voice and one practical/end-user voice.

### Step 3: Provide Context Brief

Write a brief (3-5 sentences) that all panelists share. This ensures everyone argues from the same factual base. Include what the thing is, what it does, who it serves, and any non-negotiable constraints.

## The Deliberation Protocol

### Round 1: Proposals (divergent)

Each panelist proposes 3-5 candidates with brief rationale (one sentence each). The goal is breadth. Panelists should draw from their unique perspective. Expect 20-35 total proposals.

Present as a numbered list under each panelist's name.

### Round 2: Discussion (convergent)

Each panelist reviews all Round 1 proposals and comments on 3-5 that caught their attention (positively or negatively). They should argue from their expertise: a clinical user critiques usability, a journal editor critiques searchability, a tech visionary critiques scalability.

Identify emerging consensus and genuine disagreements. Surface the 5-8 strongest candidates.

### Round 3: Independent Voting

Each panelist ranks their top 3 choices (3 points, 2 points, 1 point). Voting must be independent: present each panelist's vote without reference to others. Tally the scores.

Present a clear scoreboard.

### Round 4: Responsive Discussion (if needed)

If the user pushes back on the leading candidate or introduces a new constraint, run a focused round where panelists respond to the specific concern. This round often produces the best candidates because it combines collective intelligence with the user's domain insight.

### Round 5: Final Convergence (if needed)

One more independent vote on the refined shortlist. Present the final recommendation with primary choice, alternative, and simple/informal variant.

## Key Principles

**Independence matters.** Each panelist must vote based on their own reasoning, not swayed by the group. This is what makes the technique work. If everyone just agrees with the loudest voice, you get a worse result than thinking alone.

**Respect the user's constraints.** When the user introduces new information or constraints mid-discussion, treat it seriously. The panel serves the user, not the other way around. Pivot the discussion to address their concern directly.

**Diversity of perspective, not diversity for its own sake.** Each panelist should bring a viewpoint that would genuinely change the decision if taken seriously. Remove panelists whose perspective overlaps too much with another's.

**Real personality.** Write each panelist's contributions in a voice consistent with their expertise and temperament. A journal editor sounds different from a tech visionary. This is not theater, it is functional: distinct voices surface distinct concerns.

**Convergence is not forced.** If the panel genuinely cannot agree, report that honestly. A split decision with clear reasoning is more useful than a false consensus.

## Output Format

Structure the output as a markdown document with clear round headers, panelist names, and a final recommendation section. If the user wants it saved, write to a file in the project directory.

```markdown
# [Decision Topic] — Panel Discussion

## Context
[Brief]

## Panelists
[Names and roles]

---

## Round 1: Proposals
### [Panelist Name]
1. **Candidate** — Rationale
...

## Round 2: Discussion
### [Panelist Name]
[Commentary on other proposals]
...

## Round 3: Independent Voting
| Panelist | 1st (3pt) | 2nd (2pt) | 3rd (1pt) |
...

### Scoreboard
...

## Round 4: [If triggered by user feedback]
...

## Round 5: Final Vote
...

---

## Final Recommendation
**Primary:** ...
**Alternative:** ...
**Simple variant:** ...
```

## Integration with Other Skills

This skill is standalone. It can be invoked by the user directly, by the scope-strategist (for framing decisions), by the paper-architect (for structural choices), by the crystalit orchestrator (for ontology decisions), or by any other skill that faces a decision benefiting from diverse reasoning.

When invoked by another skill, that skill provides the context brief and constraints. The panel-discussion skill runs the deliberation and returns the recommendation.
