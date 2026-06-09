# Interview Prep Generator — Skill

Produces a complete, self-contained HTML interview prep document tailored to a specific role, company, and your background. One file you open in any browser: sidebar navigation, expandable Q&A, a pre-flight checklist that saves your progress, and print-to-PDF. Works offline once downloaded.

## What's in this bundle

- **`interview-prep-generator.skill`** — the installable skill package.
- **`interview-prep-generator/`** — the unpacked skill (same contents as the `.skill`, for inspection or editing).
- **`sample_interview_prep_cobalt_senior_pm.html`** — an example output (fictional candidate + JD) so you can see what to expect.

## Install

Import `interview-prep-generator.skill` into your Claude environment (the exact step depends on your Claude tier/product). Once installed, the skill triggers automatically when you ask for interview prep or paste a job description for an upcoming interview.

## Set up your Claude Project (one time, for best results)

The skill runs inside a Claude Project so your materials persist across many prep runs — only the job description changes each time.

**Required**
- Your resume (`.docx`, `.pdf`, or `.txt`)

**Strongly recommended** (the output gets much more personal)
- A reference doc covering your key accomplishments, metrics, flagship stories, and writing voice
- 1–2 prior interview prep docs that worked (used as voice/style templates)

**Optional** (used when present)
- LinkedIn recommendations or peer testimonials
- Writing samples in your voice (articles, posts, essays)
- Companion documents you want linked from the brief (case studies, portfolio pieces)
- Notes on the interviewer if you know who it is

You don't have to set everything up first. If your project is empty or sparse, the skill detects that and walks you through uploading what's needed before producing the brief.

## Use it

1. Open your Claude Project.
2. Paste or upload the job description.
3. Say *"Make me interview prep for this role."*
4. Answer any quick questions (format, interviewer, etc.).
5. Download the resulting HTML and open it in a browser.

**In the document:** sidebar nav jumps between sections · press **E** to expand all answers, **C** to collapse · the right-rail checklist saves to your browser · press **P** to print or save as PDF.

## Tips

- The richer your project content, the more the output sounds like *you* rather than generic AI. A resume alone gives generic results; resume + reference doc + recommendations + writing samples produces output in your voice.
- Give the interviewer's name and role (recuiter, hiring manager, etc.) when you have it — it gets used in the brief.
- Want a light or brand-colored theme? Mention it when you invoke.
- The skill never invents metrics or claims. If your record doesn't back a JD requirement, it flags the gap honestly instead of faking it — fill those before the interview.
- After the interview, drop notes back into your project so future preps benefit from what you learned.
