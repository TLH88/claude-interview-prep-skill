---
name: interview-prep-generator
description: Generates a complete, structured interview preparation HTML document for a specific role at a specific company. Use this skill whenever the user wants to create interview prep, build an interview brief, prepare for an upcoming interview, or generate STAR answers for a job they have an interview scheduled for. Also trigger when the user uploads or pastes a job description and mentions an upcoming interview, screen, or hiring conversation. Trigger even when the user does not explicitly say "interview prep" but the context makes the intent clear: pasting a JD, mentioning an interviewer by name, describing a screening conversation, or asking for help with a specific role they're pursuing. Produces a single standalone HTML file the user can open in a browser, with sidebar navigation, expandable Q&A blocks, a pre-flight checklist, and keyboard shortcuts. The document is self-contained and works offline once downloaded.
---

# Interview Prep Generator

You produce a single, self-contained HTML interview prep document for a specific candidate, role, and company. The user has typically given you a job description and may have additional context in the Claude Project. The output is one file the candidate can open in a browser, reference live during the call, and check items off as they go.

The structure is fixed; the content is not. Every fact, metric, and recommendation comes from the candidate's materials and the JD — never from invention. When the proof isn't there, you flag the gap honestly. A structurally perfect brief in generic LLM voice is a failure, because the candidate rewrites all of it. Getting the voice right matters more than filling every section.

## What to do first

### Step 1: Detect project state and onboard if needed

Before anything else, check what candidate materials exist. Use `project_knowledge_search` (or the equivalent retrieval available to you) to look for: a resume or CV, a reference document about the candidate's experience, prior interview prep examples, recommendation letters or testimonials, writing samples.

Three states are possible:

- **State A — Rich project** (resume + reference doc + extras present): proceed to Step 2.
- **State B — Minimal project** (resume only, or close to it): in a single message, note what's missing and offer a choice: *"I can produce prep now with limited personalization, or you can take ~10 minutes to upload a reference doc, recommendations, or writing samples and the output will be much stronger. Which do you prefer?"* Then act on the answer.
- **State C — Empty project** (no candidate materials): switch to onboarding. In one consolidated message, ask the user to upload (1) their resume — required to proceed, (2) a reference doc or summary of key accomplishments and stories — strongly recommended, (3) any recommendations they want available for credibility callouts — optional, (4) any writing samples showing their voice — optional. Once they've uploaded what they want (or said to proceed as-is), continue.

Do not loop or re-ask. One consolidated prompt, then act.

### Step 2: Capture the interview inputs

Confirm you have:

1. **The job description.** URL → fetch it. Pasted text → use it. File → read it. **If the JD is missing, ask for it and stop. Do not produce prep without one.**
2. **The interview context.** Ask once, only if not provided: who is the interviewer, what's the format (recruiter screen / hiring manager / panel), the duration, and any known date/time. Missing context is fine — the relevant template fields drop out cleanly (see `references/variability-handling.md`).

### Step 3: Decide section scope

Read `references/variability-handling.md` and decide which sections are in.

- **Always include:** Hero, Company context (§01), Requirements-to-proof matrix (§03), Q&A (§04), Questions to ask (§05), Watch points (§06), Pre-flight checklist.
- **Conditional:** Values mapping (§02 — only if the company publishes named values), Research-insights card (only if you did web research), Related-docs launcher (only if the candidate has companion documents to link), Recommendation callouts inside Q&A/values (only if the candidate uploaded recommendations).

### Step 4: Research the company (on by default)

Web research is enabled by default. Unless the user opts out:

- Look up the company's published values, mission, recent news, and any publicly visible interview questions (Glassdoor, Reddit, Blind).
- If the interviewer's name is known, look them up for role-relevant context only — never put personal details in the output.
- Look up recent leadership statements, blog posts, or product announcements that signal priorities.

Cap at ~10–15 searches. Signal, not exhaustiveness. Any specific interview question you confirm from a public source becomes a high-priority Q&A entry, tagged `Confirmed [Company] Q`.

### Step 5: Build the proof matrix

Read the matrix section of `references/section-guide.md`. For each JD requirement, find the candidate's strongest evidence. If a requirement has no clear match, do not invent one — either mark it an honest gap (use the gap row style) or omit the row. Never claim experience the candidate lacks.

### Step 6: Generate the Q&A bank

Use `references/question-bank.md` as the starting list. For each likely question, write a prepared answer from the candidate's actual experience, not generic advice. Behavioral questions → STAR rows (Situation/Task/Action/Result). Opener and philosophy questions → general rows (Approach/Script/Frame/etc.). Aim for **6–10** answers (lean 6 for a 30-min screen, 10 for hiring-manager or panel rounds). Tag confirmed company questions `Confirmed [Company] Q` and mark the single most important answer `High priority`.

### Step 7: Generate the remaining section content

Company brief cards (1–3, plus the optional research card), values mapping if applicable, questions to ask, watch points (4–8, color-coded), and the checklist (start from `assets/checklist-defaults.json`, then add candidate-specific items).

### Step 8: Apply voice and tone

Read `references/voice-and-tone.md` **before writing any prose** and keep it in mind throughout. Mirror the candidate's writing samples when present; otherwise default to a direct, executive tone. No consultant-speak, no LLM tells, no em dashes (unless the user overrides).

### Step 9: Fill the template

Read `assets/template.html`, replace every `{{PLACEHOLDER}}`, and resolve conditional sections (see "Filling the template" below). Apply the theme via `{{THEME_CSS_OVERRIDES}}` — empty for the default dark theme.

### Step 10: Validate, then save and present

Run the validation checklist below. Then save as `interview_prep_[company]_[role].html` (sanitized for the filesystem) and present it with a short summary plus any honest caveats (e.g., *"No published values found for this company, so the Values section was omitted,"* or *"Two matrix rows are gaps — fill these before the interview."*).

---

## Filling the template

The template is `assets/template.html`. It is a generalization of the proven reference brief: identical CSS, layout, and JavaScript; only the content is parameterized. Two mechanisms:

1. **`{{PLACEHOLDER}}`** — replace with generated content.
2. **`<!-- BEGIN_OPTIONAL_X -->` … `<!-- END_OPTIONAL_X -->`** — if section X's content is empty, delete everything between the markers **including the markers**. If X is kept, just delete the two marker comment lines and keep the content.

### Placeholder reference

**Identity / meta:**

| Placeholder | Fill with | Example |
|---|---|---|
| `{{COMPANY_NAME}}` | Company name | `Zapier` |
| `{{ROLE_TITLE}}` | Full role title | `Sr. Manager, Global Support` |
| `{{ROLE_SHORT}}` | Short role for the sidebar brand | `Sr. Manager` |
| `{{SIDEBAR_BRAND_SUB}}` | One line under the brand | `Global Support · 30-min screen with Malia` |
| `{{HERO_EYEBROW}}` | Hero eyebrow | `Interview preparation · Confidential` |
| `{{HERO_TITLE_LEAD}}` / `{{HERO_TITLE_EM}}` | Hero title split; EM is the italic-blue part | `Sr. Manager,` / `Global Support` |
| `{{HERO_SUBTITLE}}` | One-line context about the interview | `A 30-minute recruiter screen at Zapier with Malia, focused on…` |
| `{{INTERVIEW_FORMAT}}` | Format meta value | `30-min Zoom` |
| `{{INTERVIEWER_NAME}}` | Interviewer + tz (OPTIONAL_INTERVIEWER) | `Malia (EST)` |
| `{{COMP_RANGE}}` | Comp meta value (OPTIONAL_COMP_DETAILS) | `$130.8K – $196.2K` |
| `{{HOURS_OR_LOCATION}}` | Hours/location meta value | `PST core, remote` |
| `{{ASK_NAV_LABEL}}` | Sidebar nav label for §05 | `Ask Malia` or `Questions to ask` |
| `{{STORAGE_SLUG}}` | localStorage namespace, kebab-case, unique per doc | `zapier-srmgr-globalsupport` |
| `{{FOOTER_LINE}}` / `{{FOOTER_SUB}}` | Sign-off | `Good luck out there, Tony.` / `The proof is in your record…` |

**Section content:**

| Placeholder | Fill with |
|---|---|
| `{{COMPANY_LEDE}}` | §01 lede, 1–2 sentences on what the company is now and why the role exists |
| `{{COMPANY_BRIEF_CARDS}}` | 1–3 `.brief-card` blocks (see snippet) |
| `{{RESEARCH_INSIGHTS_CARD}}` | One richer `.brief-card` of research findings (OPTIONAL_RESEARCH_INSIGHTS); appended inside the brief grid |
| `{{COMPANY_PULL_QUOTE}}` / `{{COMPANY_PULL_CITE}}` | A short verbatim quote from the JD/company + its citation |
| `{{VALUES_TITLE}}` / `{{VALUES_LEDE}}` / `{{VALUES_GRID}}` | §02 (OPTIONAL_VALUES): heading, lede, and `.value-card` blocks |
| `{{MATRIX_LEDE}}` / `{{MATRIX_HEAD_LEFT}}` / `{{PROOF_MATRIX_ROWS}}` | §03 lede, left header (`What [Company] wants`), and `.matrix-row` blocks |
| `{{ANSWERS_LEDE}}` / `{{QA_BLOCKS}}` | §04 lede + `.q-block` blocks |
| `{{ASK_TITLE}}` / `{{ASK_LEDE}}` / `{{ASK_INTERVIEWER_LIST}}` | §05 heading (`Questions to ask Malia`), lede, and the ask groups |
| `{{WATCH_LEDE}}` / `{{WATCH_POINTS}}` | §06 lede + `.watch-card` blocks |
| `{{CHECKLIST_GROUPS}}` / `{{CHECKLIST_TOTAL_COUNT}}` | The `.check-group` blocks and the total item count |
| `{{RELATED_DOCS_BUTTONS}}` / `{{RELATED_DOCS_NOTE}}` / `{{RELATED_DOCS_MAP}}` / `{{RELATED_DOCS_SHORTCUTS}}` | OPTIONAL_RELATED_DOCS: sidebar buttons, note, JS filename map, and keydown handlers |
| `{{THEME_CSS_OVERRIDES}}` | Empty for dark; else CSS var overrides (see `assets/theme-tokens.css`) |

### Component snippets

Use these exact shapes when filling the repeating placeholders.

**Brief card** (`{{COMPANY_BRIEF_CARDS}}`, and the research card):
```html
<div class="brief-card">
  <div class="brief-card-icon">Z</div>
  <h3 class="brief-card-title">What they are now</h3>
  <p class="brief-card-body">One tight paragraph. Use <strong>strong</strong> for the load-bearing phrase and <em>em</em> sparingly for the one detail that reframes things.</p>
</div>
```

**Value card** (`{{VALUES_GRID}}`):
```html
<div class="value-card">
  <div class="value-card-num">VALUE 01</div>
  <div class="value-card-name">Default to action</div>
  <p class="value-card-proof"><strong>The candidate's proof.</strong> One or two sentences of concrete evidence that maps to this value.</p>
</div>
```

**Matrix row** (`{{PROOF_MATRIX_ROWS}}`) — normal and gap variants:
```html
<div class="matrix-row">
  <div class="matrix-cell">A JD requirement as a noun phrase</div>
  <div class="matrix-cell">One or two sentences of specific proof: metrics, scope, role, dates.</div>
</div>
<div class="matrix-row gap">
  <div class="matrix-cell"><span class="gap-badge">Gap</span>Requirement with no clean match</div>
  <div class="matrix-cell">Name it honestly and note the nearest adjacent experience. Do not fabricate.</div>
</div>
```

**Q&A block** (`{{QA_BLOCKS}}`) — tag is plain, or add `priority` class for the key/confirmed ones:
```html
<div class="q-block">
  <div class="q-header" onclick="toggleQ(this)">
    <div class="q-num">Q1</div>
    <div class="q-text">Why this company? Why this role?</div>
    <div class="q-tag">Opener</div>
    <div class="q-chevron">▾</div>
  </div>
  <div class="q-body"><div class="q-body-inner">
    <div class="star-row">
      <div class="star-label s">Situation</div>
      <div class="star-content">…</div>
    </div>
    <!-- star-label kinds: s, t, a, r (STAR) or gen (Approach/Script/Frame/Delivery notes) -->
  </div></div>
</div>
```

**Watch card** (`{{WATCH_POINTS}}`) — class is `high`, `medium`, or `low`:
```html
<div class="watch-card high">
  <div class="watch-priority">⬤ High priority</div>
  <h3 class="watch-title">Short title</h3>
  <p class="watch-body">What to manage and how. <strong>Bold</strong> the action.</p>
</div>
```

**Ask group + item** (`{{ASK_INTERVIEWER_LIST}}`) — repeat the group title + list; `hold` items are for a later round, `gold` titles for the hold group:
```html
<h3 class="ask-group-title">Recruiter wheelhouse</h3>
<p class="ask-group-note">Questions this interviewer can actually answer. Prioritize these.</p>
<div class="ask-list">
  <div class="ask-item">
    <div class="ask-num">01</div>
    <div class="ask-text"><strong>Lead phrase?</strong> The rest of the question.</div>
  </div>
</div>
```

**Checklist group** (`{{CHECKLIST_GROUPS}}`) — `data-group` unique per group, `data-key` unique per item across the whole doc:
```html
<div class="check-group" data-group="prep">
  <button class="check-group-head" onclick="toggleCheckGroup(this)">
    <span class="cg-chevron">▾</span>
    <span class="cg-title">24 hours before</span>
    <span class="cg-progress">0/6</span>
    <span class="cg-done">✓</span>
  </button>
  <div class="check-group-items">
    <label class="check-item"><input type="checkbox" data-key="prep1"><span class="check-label">An item</span></label>
  </div>
</div>
```

**Related docs** (OPTIONAL_RELATED_DOCS) — only when the candidate names companion files to link. Assign a single-letter shortcut to each (avoid E, C, P, which are reserved). Buttons:
```html
<button class="sidebar-btn primary-gold" onclick="openDoc('workflows')" title="Workflow diagrams">
  <span><span class="doc-icon">◐</span>Workflow diagrams</span><span class="sidebar-btn-kbd">W</span>
</button>
```
`{{RELATED_DOCS_MAP}}` is a JS object literal of candidate filenames, e.g.
`{ workflows: ['zapier_workflow_diagrams.html'], research: ['zapier_research_brief.html'] }`.
`{{RELATED_DOCS_SHORTCUTS}}` is the matching keydown lines, e.g.
`if (e.key === 'w' || e.key === 'W') openDoc('workflows');`

### Section numbering

The template ships numbered with Values at §02. **If you omit the Values section, renumber** the remaining sidebar nav nums and `.section-num` values so they stay sequential (Company 01, Matrix 02, Q&A 03, Ask 04, Watch 05) and drop the `02 Values match` nav link with the rest of the OPTIONAL_VALUES block. Leaving `'values'` in the scroll-spy `sections` array is harmless (it's null-guarded).

---

## Validation before delivering

- Every `{{PLACEHOLDER}}` is replaced; no `{{` remains in the output.
- Every `BEGIN_OPTIONAL_` / `END_OPTIONAL_` marker is gone — either the block was removed entirely, or just the two comment lines were stripped.
- HTML tags balance; no orphaned `<div>`.
- The rail header `0 / N`, the FAB `0/N`, and each group's `x/N` all equal the real item counts.
- Every `data-key` is unique; `{{STORAGE_SLUG}}` is set and unique to this doc.
- All `href="#id"` anchors point to sections that still exist.
- Section numbers are sequential after any omission.
- **No invented facts, metrics, names, or recommendations.** Every claim traces to the candidate's materials, the JD, or a cited research source. Gaps are flagged, not filled with fiction.

## Working with limited inputs

JD only, no candidate background: still produce the document, but make the missing proof explicit with bracketed prompts in the matrix and Q&A, e.g. `[INSERT YOUR PROOF POINT — maps to the requirement on the left]`. State clearly in your delivery message what's missing and that the user must fill it before the interview.

## Working with rich project knowledge

Lean on it. Quote specific metrics, attribute recommendations to author and role, and pull voice patterns from prior writing samples and prep docs. The richer the input, the more the output should sound like the candidate, not like Claude.

## Voice and tone (always)

Read `references/voice-and-tone.md` first. Most common failure: generic consultant-speak. Avoid "sits at the intersection of," "captured the pattern," "leverage" as a verb, em dashes, and vague openers like "I'm passionate about" or "I bring a unique blend of." Ground every opener in concrete experience.
