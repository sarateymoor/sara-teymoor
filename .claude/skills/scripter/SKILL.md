---
name: content-scripter
description: Write video scripts using proven hook patterns and structures. Handles short-form (TikTok, Reels, Shorts) and long-form (YouTube, VSLs). Reads scripting frameworks from references/ before writing. Use when scripting content for any brand or format.
---

# Content Scripter — Interactive Filming Card Generator

Take a topic → present hook options → generate a filming card the creator riffs from on camera.

**Your scope:** Topic + hook selection → filming card → save to pipeline
**NOT your scope:** Finding topics (/daily-content-researcher), generating ideas (/content-ideator)

---

## Before EVERY Session — Mandatory Reads

Read ALL THREE before writing anything:

1. **`references/framework.md`** — Kallaway hook psychology, 3-beat rule, validation checklist (lives in this skill)
2. **Project voice guide** — Speech patterns, language rules, winner/loser examples (path from project CLAUDE.md)
3. **Project hook swipe file** — Proven frameworks with real performance data (path from project CLAUDE.md)

If the project CLAUDE.md doesn't specify voice or hook files, use the defaults in `references/framework.md`.

---

## Step 0: Read Research from Pipeline

Before anything else, check if this topic has research in the Content Pipeline.

**Airtable Details:**
- MCP: your Airtable MCP → `search_records`
- Base: Read Base ID and Table ID from your project's CLAUDE.md → Data Sources section
- Table: Read Base ID and Table ID from your project's CLAUDE.md → Data Sources section (Content Pipeline)
- Filter: Match by topic title, status "Researched"

**Read these fields:**
- `Content Angle` — the chosen angle from ideation
- `Materials & Research` — full research brief (key facts, sources, demo details, what everyone else is saying, the gap)
- `Type` — content type determined during ideation (Demo, Tutorial, Everything Else)

**If research exists:** Use it to inform hook selection AND filming card details. The research brief contains specific facts, numbers, and names — use these instead of generic placeholders.

**If no research exists:** Proceed normally (the topic may have been given directly without going through the pipeline).

---

## Step 0b: Confirm Content Type

The content type should already be set during ideation. Confirm it with the user. Default types:

| Type | When | Examples |
|------|------|---------|
| **Demo** | Showcasing a tool, workflow, automation, build, or someone else's project | n8n workflows, Claude builds, app demos, split-screen showcases |
| **Tutorial** | Step-by-step teaching how to do something | Product walkthroughs, feature explanations, how-tos |
| **Everything Else** | Hot takes, reacts, news, comparisons, lists, humor | Opinions, reactions, breaking news, comparisons |

**For demos with workflows:** If n8n MCP tools are available and the topic involves a workflow, fetch the actual workflow JSON first. Extract: What triggers it? What's the input? What AI processes it? What's the output? Use these real details in the filming card.

---

## Step 1: Present 6 Hook Options (WAIT for response)

**DO NOT skip this step. Present hooks and WAIT for the user to pick one.**

### How to build hooks:

1. Query the Supabase `content` table for analyzed top performer hooks that are relevant to this topic and content type
2. Pull 6 hooks — show the **spoken hook** AND the **spoken hook framework** for each
3. Apply the creator's voice patterns from the voice guide
4. Mix: **2 contrarian hooks** + **4 best fit** (educational, raw shock, secret reveal, comparison — whatever matches)

### Supabase Query:

```sql
SELECT spoken_hook, spoken_hook_framework, spoken_hook_structure,
       handle, views, url
FROM content
WHERE spoken_hook IS NOT NULL
  AND spoken_hook != ''
  AND spoken_hook_structure IN ('[relevant structures]')
ORDER BY views DESC
LIMIT 20
```

Pick the 6 most relevant from the results. For the 2 contrarian hooks, filter for `spoken_hook_structure = 'Contrarian/Negative'`.

If not enough results from `content`, supplement from `kallaway_hooks` table.

### Performance insights to apply (from latest analysis):

- **Shorter hooks win.** Fewer words = more views. Under 15 words ideal.
- **Withhold the payoff.** "Look at this" (curiosity) beats "I just built X" (reveal). Don't front-load what the viewer will see.
- **Educational/Tutorial hooks outperform Raw Shock.** "I'm going to teach you" (61k avg) beats "This is insane" (32k avg).
- **"Teach" > "Show"** in hook language — it implies the viewer learns something.
- **Time constraints create urgency.** "in the next 30 seconds" outperformed "in the next 60 seconds" by 7.5x on the same video.

### Rules for hooks:

- Under 15 words (shorter = more views)
- Use the creator's actual speech patterns (from voice guide)
- Topic clarity in first 2 seconds
- Withhold the payoff — create a curiosity gap, don't reveal what they'll see
- Each hook uses a DIFFERENT framework/structure

### Present format:

```
## 6 Hook Options for: [Topic]
**Content Type:** [Demo / Tutorial / Everything Else]

**A)** "[Adapted hook text]"
   Framework: [spoken_hook_framework with [X] [Y] placeholders]
   Source: @[handle] — [X] views | Structure: [type]

**B)** "[Adapted hook text]"
   Framework: [spoken_hook_framework]
   Source: @[handle] — [X] views | Structure: [type]

**Pick one or more (e.g., A, C, E) — each becomes its own video. Or edit / write your own.**
```

**STOP HERE. Wait for user to choose before generating the filming card.**

---

## Step 2: Generate Filming Card

Based on the chosen hook + content type, generate a filming card in **sequential format**.

### Card Format (All Content Types)

```
HOOK: [chosen line]

[Framework for this beat]
"[Example sentence in your voice]"

[Framework for next beat]
"[Example sentence in your voice]"

CLOSE: "[CTA or emotional payoff — 1 line]"
```

---

## Step 3: Save to Pipeline

After the user approves the filming card(s), save to Airtable.

**Airtable Details:**
- MCP: your Airtable MCP → `update_records` (single hook) or `create_record` (multiple hooks)
- Base: Read Base ID and Table ID from your project's CLAUDE.md → Data Sources section
- Table: Read Base ID and Table ID from your project's CLAUDE.md → Data Sources section (Content Pipeline)

After saving script fields, update `Status` to "Filming" to move the record into the filming pipeline.
