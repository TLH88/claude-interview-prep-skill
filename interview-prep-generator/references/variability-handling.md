# Variability Handling

The decision tree for adapting the template to whatever inputs exist. The structure is fixed; what's included and how it's written flexes. Default behavior is listed first for each decision.

## Conditional sections and their markers

Each optional block in `template.html` is wrapped in `<!-- BEGIN_OPTIONAL_X -->` / `<!-- END_OPTIONAL_X -->`. Keep it: delete only the two marker lines. Drop it: delete everything between the markers, inclusive.

| Marker | Drop the block when | If kept |
|---|---|---|
| `OPTIONAL_VALUES` | Company publishes no named, discoverable values | §02 + its nav link; **renumber** following sections |
| `OPTIONAL_RESEARCH_INSIGHTS` | No web research was done / nothing material found | The richer 4th brief card |
| `OPTIONAL_RELATED_DOCS` | Candidate named no companion documents to link | Sidebar buttons, note, JS map, and W/R/V-style shortcuts |
| `OPTIONAL_COMP_DETAILS` | No comp info available | The "Comp" meta row |
| `OPTIONAL_INTERVIEWER` | Interviewer unknown | The "With" meta row |

When a block is dropped, also clean up anything that referenced it (e.g. a checklist item that says "review each panelist" makes no sense if there's no panel). The scroll-spy `sections` array is null-guarded, so leaving a removed id in it is harmless — but renumber the visible nav nums and `.section-num`s so they stay sequential.

---

## The seven decisions

### 1. Should I do web research?
**Default: yes.** Web research is on by default. Do it whenever web access exists and the company is identifiable. Skip only if the user opts out, or the company isn't a discoverable entity (tiny/private/stealth). Cap ~10–15 searches. If you skip or come up empty, drop `OPTIONAL_RESEARCH_INSIGHTS` and note it in delivery.

### 2. Should I include the Values section?
**Default: only if earned.** Yes if the company has named, published values you can confirm in 1–2 searches (and ideally evidence they surface in interviews). No otherwise — drop `OPTIONAL_VALUES` and renumber. Don't invent values or stretch generic mission language into a "values" grid.

### 3. Should I include recommendation callouts?
**Default: no, unless materials exist.** Include them only if the candidate uploaded LinkedIn recommendations or peer testimonials. When included, use them selectively — only where external validation earns its place (a values proof, a coaching result) — and attribute by author name and role, in their actual words. Never paraphrase a recommendation into something stronger than it said.

### 4. Should I include the Related Docs launcher?
**Default: no.** Include it only if the user explicitly names companion documents they want linkable (workflow diagrams, a research brief, case studies). Then ask once which files and which shortcut letters (avoid E/C/P, which are reserved), and fill the buttons, JS map, and shortcuts. Otherwise drop `OPTIONAL_RELATED_DOCS` entirely. Add the "keep files in the same folder" note only when the launcher is present.

### 5. What checklist items?
**Default: defaults + specifics.** Start from `assets/checklist-defaults.json`. Substitute `[COMPANY]`/`[INTERVIEWER]`/`[ROLE]`. Then add items derived from this brief: a question to rehearse, a number to confirm, a line to slow down on, a panelist to review. Recount every total (rail header, FAB, per-group `x/N`, `{{CHECKLIST_TOTAL_COUNT}}`). Keep `data-key`s unique.

### 6. What Q&A count?
**Default: 6–10.** Lean **6** for short recruiter screens (30 min), **10** for hiring-manager rounds, **10–12** for panels. Always include the opener; always include confirmed company questions.

### 7. What theme?
**Default: dark.** Leave `{{THEME_CSS_OVERRIDES}}` empty. If the user asks for light or a brand accent, emit overrides per `assets/theme-tokens.css`. Brand colors may use the **candidate's** identity only, never the target company's.

---

## Input edge cases

### JD only, no candidate background
Still produce the document. In the matrix and Q&A, replace missing proof with bracketed prompts: `[INSERT YOUR PROOF POINT — maps to the requirement on the left]`, `[YOUR METRIC HERE]`. Keep the structure so the candidate sees exactly what to fill. State plainly in delivery what's missing and that they must complete it before the interview. Do not invent the proof.

### Company has no public web presence
Drop `OPTIONAL_VALUES` and `OPTIONAL_RESEARCH_INSIGHTS`. Build the matrix and Q&A entirely from the JD. Note the limitation in delivery.

### Candidate has multiple companion documents
Enable `OPTIONAL_RELATED_DOCS`. Confirm which docs and shortcut keys once. Generate buttons + JS map + shortcuts to match. Primary/most-used doc can take the `primary-gold` button style.

### Non-dark theme requested
Apply `{{THEME_CSS_OVERRIDES}}`. Light = invert surfaces and ink (recipe in `theme-tokens.css`). Brand = re-point the `--blue` spine to the candidate's accent.

### Panel, not a single conversation
Q&A goes to 10–12. Hero subtitle and brand-sub mention the panel. Add checklist items like "review each panelist's background." Watch points reflect multiple evaluators.

### Recruiter screen vs. hiring manager round
- **Recruiter screen:** emphasize opener, why-this-role, comp/logistics, process questions. Fewer deep STAR answers. §05 "wheelhouse" questions are recruiter-answerable; push strategic ones to the hold group. Watch points: "this is a screen, get to the next round."
- **Hiring manager round:** emphasize behavioral STAR depth and domain expertise. §05 strategic questions move into play. Watch points reflect technical/strategic evaluation.

### Non-English company or JD in another language
Work from the JD as given. Write the brief in the candidate's working language (English by default) unless asked otherwise. If you can't reliably parse the JD, say so and ask for a translation or the key requirements rather than guessing.

### Different domain than the project's defaults
Don't overfit to the project's apparent specialty. Derive the matrix and Q&A from *this* JD's requirements. Pull from whichever parts of the candidate's record actually map, and flag genuine domain gaps honestly rather than forcing the old domain's stories onto a new role.
