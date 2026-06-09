# Section Guide

What goes in each section of the brief, the inputs it needs, length targets, failure modes, and good/bad examples. Read the relevant section before writing it. Examples below are drawn from or modeled on the reference brief.

The document order is fixed: Hero → §01 Company → §02 Values (optional) → §03 Proof matrix → §04 Q&A → §05 Questions to ask → §06 Watch points → Footer, with the pre-flight checklist in the right rail.

---

## Hero

**Purpose:** Orient the candidate in two seconds — what role, where, what shape of conversation.

**Inputs:** Role title, company, interview format/interviewer.

**Fill:** `{{HERO_TITLE_LEAD}}` + `{{HERO_TITLE_EM}}` split the role so the back half renders italic-blue (e.g. `Sr. Manager,` / `Global Support`). `{{HERO_SUBTITLE}}` is one sentence naming the format, the interviewer, and the 2–4 things the conversation will actually test.

**Good subtitle:** "A 30-minute recruiter screen at Zapier with Malia, focused on background fit, leadership experience, navigating ambiguity in scale-up environments, and hands-on AI usage."

**Failure modes:** Generic ("Get ready to ace your interview!"). Listing everything instead of the few things this round tests.

---

## §01 Company, in context

**Purpose:** Show the candidate understands what the company *is now* (not its founding myth) and why this role exists right now. This is the framing they'll speak from.

**Inputs:** JD, company research, the candidate's relevant adjacencies.

**Structure:** A lede (1–2 sentences with a point of view, not a description), then 1–3 `.brief-card`s, an optional research-insights card, and a pull quote.

- **Brief cards** each take a title + one paragraph. Good titles are angles, not labels: "What they are now," "How they actually work," "Why this role exists." Where a company fact lines up with the candidate's background, say so in the card ("…fully async since 2011. Your Comscore Director tenure was structured the same way.").
- **Research-insights card** (optional) is the one richer card: specific, sourced findings that change how the candidate should play the call — a named leader's known habit, a value that was recently rewritten, a confirmed comp practice. Use `<strong>` and `<em>` to make the load-bearing facts pop. Omit the card entirely if no research was done.
- **Pull quote** is a short verbatim line from the JD or company materials, with a citation ("From the [Company] job description, verbatim"). Don't paraphrase and call it a quote.

**Length:** Cards ~2–4 sentences. Lede under 40 words.

**Failure modes:** Reciting the company's marketing. Cards with no connection to the candidate. A "quote" that's actually invented.

---

## §02 Values, mapped to proof (optional)

**Purpose:** When a company publishes named values that surface in screens, pre-load the candidate's single best example for each so they can summon it without searching.

**Include only if** the company has discoverable, named values (1–2 searches). Otherwise omit the whole section and renumber.

**Inputs:** The published values (use the company's exact names), the candidate's record.

**Structure:** Lede, then one `.value-card` per value: `VALUE 0N` label, the value name, and one or two sentences of the candidate's concrete proof. Star or emphasize the value that is the candidate's strongest differentiator. If a recommendation backs a value, attribute it by name.

**Good proof:** "**ConMet zero-to-one.** Built support from nothing on a hard deadline with no playbook. Moved daily without full information." (maps to "Default to action")

**Failure modes:** Restating the value instead of proving it. Forcing a weak example for a value the candidate can't actually evidence — better to make that card honestly lighter than to fake it.

---

## §03 Requirements to proof

**Purpose:** Make the JD-to-candidate alignment explicit and scannable in 30 seconds.

**Inputs:** The full JD (or its key requirements as noun phrases) + the candidate's background.

**Structure:** Two-column matrix, header left = `What [Company] wants`, right = `Your strongest proof`. **7–10 rows.** Each left cell is one requirement as a noun phrase. Each right cell is one or two sentences of specific evidence: metrics, scope, role, dates.

**Length:** Each row readable in under 8 seconds. If the proof needs more room, it belongs in a Q&A answer, not here.

**Gaps:** If a requirement has no clean match, either omit the row or render it as a `.matrix-row gap` with a `<span class="gap-badge">Gap</span>`, naming the gap honestly and the nearest adjacent experience. Never fabricate a row to look complete.

**Example (good):**
- Requirement: "5+ years leading distributed support teams"
- Proof: "6+ years SaaS support leadership across Comscore (70 FTE, U.S./LATAM/India) and ConMet zero-to-one build (Dec 2018 – May 2025)."

**Example (bad — too generic):**
- Requirement: "Strong leadership skills"
- Proof: "Demonstrated leadership across multiple roles."

**Failure modes:** Generic claims with no evidence. Conflating "would like to do" with "have done." Inventing numbers to fill a row. Padding past the JD's actual requirements.

---

## §04 Likely questions, with answers

**Purpose:** The core of the brief. Pre-built answers in the candidate's voice so they speak from preparation, not improvisation.

**Inputs:** `references/question-bank.md`, the candidate's stories and metrics, any confirmed company questions from research.

**Structure:** A lede that orients (how many answers, which is the opener, which is highest-stakes, which are confirmed company questions), then **6–10** `.q-block`s.

- Each block: number, question text, a tag, chevron, and a body of `.star-row`s.
- **Behavioral** questions → STAR rows: `s` Situation, `t` Task, `a` Action, `r` Result. **Opener / philosophy / scenario** questions → `gen` rows labeled Approach, Script, Frame, Delivery notes, Why it matters, Closing line, etc.
- Tags: plain (`Opener`, `STAR`, `Career arc`, `Philosophy`) or `priority` class for the highest-stakes answer and any `Confirmed [Company] Q`.
- Use `<strong>` for the metric/claim that must land and `<em>` for the memorable line. In Action rows, structure multi-part builds with bold sub-labels or numbered branches as the reference does.

**Length:** Opener/script ~60 seconds spoken. STAR answers ~90 seconds. Don't write a monologue — if an answer sprawls, add a "Delivery strategy" gen row telling the candidate to tee up options and go deep on one.

**Order:** Opener first. Put the highest-stakes answer early (Q3–Q4). Group confirmed company questions where they're easy to find.

**Failure modes:** Answers that could belong to any candidate. Inventing a Situation. Burying the metric. Writing prose the candidate would never say out loud.

---

## §05 Questions to ask

**Purpose:** Give the candidate questions calibrated to *who is in the room*. A recruiter can't answer architecture questions; a hiring manager can.

**Inputs:** Interview format/round, interviewer role, JD specifics.

**Structure:** A lede that sets the strategy (what this interviewer can/can't answer; how many to actually ask). Then grouped `.ask-item`s:

- **Wheelhouse group** — questions this interviewer can genuinely answer. Prioritize these. 3–5 items.
- **One strategic question** — signals the candidate thinks at the system level without demanding depth the interviewer lacks. (Optional.)
- **Hold-for-next-round group** — `gold` title, `hold` items (`H1`, `H2`…) — strategic questions to save for someone with technical/team context.

Lead each item's text with a `<strong>` hook phrase. Tell the candidate how many to actually ask ("Pick 2 or 3 from the first group").

**Failure modes:** Questions easily answered by the JD. Asking the recruiter deep-strategy questions. A flat undifferentiated list with no read on the room.

---

## §06 Watch points

**Purpose:** The handful of signals to manage live — landmines, must-hit moments, and things to simply stay normal about.

**Inputs:** Everything above, plus the round type and any research red/green flags.

**Structure:** A short lede, then **4–8** `.watch-card`s, color-coded:
- `high` (red): the one or two things that decide the round — a named-in-the-invite question, a confirmed company question.
- `medium` (amber): likely themes to handle well — a culture value that will surface, how to frame a potential objection.
- `low` (green, "Manage normally"): logistics and tone — recording tools, comp positioning, "this is a screen, not a panel — the goal is the next round."

Each card: priority label, short title, one paragraph. Reference Q-numbers so the candidate can jump to the full answer ("**Q4 in your STAR set** is the most important answer").

**Failure modes:** Filler cards. Marking everything high priority (then nothing is). Vague advice with no action.

---

## Footer

`{{FOOTER_LINE}}` is a direct, warm sign-off using the candidate's name ("Good luck out there, Tony."). `{{FOOTER_SUB}}` is one grounding sentence ("The proof is in your record. Trust the work, speak from confidence."). Keep it human; no corporate boilerplate.

---

## Pre-flight checklist (right rail)

**Purpose:** A live, persistent checklist the candidate works before, during, and after the call. State saves to the browser.

**Inputs:** `assets/checklist-defaults.json`, plus specifics from this brief.

**Structure:** Grouped `.check-group`s — typically "24 hours before," "Day of," "In the call," "After." Start from the defaults, then **augment with candidate-specific items**: "Practice Q4 (AI built vs. out-of-box) out loud," "Confirm your salary target number," "Slow down on 'both unnecessary and excellent.'" Swap `[COMPANY]` / `[INTERVIEWER]` / `[ROLE]` placeholders in the defaults for the real inputs.

**Counts must match:** the rail header `0 / N`, the FAB `0/N`, and each group's `x/N` must equal the real totals. `{{CHECKLIST_TOTAL_COUNT}}` is the sum across all groups.

**Failure modes:** Mismatched counts. Generic items only (no items tied to this specific brief). Duplicate `data-key`s (breaks persistence).
