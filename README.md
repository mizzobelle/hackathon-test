# AuditPilot

**NXGN x Tandem Health Hackathon — "Clinical Admin, Reimagined"**

A clinical audit has nine steps. Exactly one of them eats your evening: reading every set of
notes against every criterion and filling in the table, one row per patient, one column per
criterion. That is the step AuditPilot automates.

AuditPilot checks **documentation against a published standard**. It never assesses clinical care.

## Run it

Open `index.html` in a browser. No build, no server, no install, no API key.

```
xdg-open index.html
```

One file. Any static host works for a public link — GitHub Pages, Netlify drop, Vercel.

## Navigating it

Five tabs across the top, not one long scroll. The header carries the running state
(`30 cases · 64% documented · 30/30 confirmed`) on every tab.

| Tab | |
|---|---|
| **Start** | Welcome page: the readiness checklist in full, how the four steps work, and what the tool will not do. |
| **Set up** | Standard, criteria, engine, case notes. Criteria and the sample cases are collapsed by default. |
| **Table** | The audit table, scrolling inside its own pane, with the evidence panel docked to the right — click a cell, the source sentence appears beside it rather than below the fold. |
| **Findings** | A 2×2 dashboard: summary + priority, compliance by criterion, ranked gaps with suggested actions, and a fourth panel that switches between **Spread**, **Together** and **Subgroup**. |
| **Report** | The builder and the draft document. |

## The 3-minute demo

1. **Start** → **Begin an audit** → **Sample cases** → **30-case sample** → **Check
   documentation**. Lands you on the table: 30 patients, 7 criteria, 210 checks, about a second.
2. Click an amber cell — *"No relevant documentation found."* The commonest real audit finding:
   the care probably happened, the notes can't prove it.
3. Click the pink **Not met** cell on Case 4 — clerking says NKDA, drug chart says penicillin
   allergy. The tool refuses to pick a side and hands it to a human.
4. **Confirm all** → **Findings**. The priority box: 3 of 7 criteria account for 67% of all gaps.
5. Switch the fourth panel to **Subgroup**: Emergency Department 34% against specialty wards 90%.
   Switch the dimension to `Admission`: out of hours 37% against in hours 82%. *That* is the
   finding — not "documentation is poor" but "fix the front door".
6. **Report** → the draft, the action plan, **Print / PDF**.

## The report — a draft that drives action

**Draft audit report** opens a two-column workspace: a **report builder** on the left, the
**document** on the right. Nothing appears in the report unless you tick it.

It is explicitly a **draft**. A banner at the top says so until every written section has been
read and marked *Checked*, and a progress bar in the builder tracks how far through you are.

**What you can change, and what you can't.**

| | |
|---|---|
| **Editable** | Introduction, Aim, Standard, Methods, Results, Discussion, Conclusion — click and type. An edited section is tagged, with a *Restore generated text* link to undo. The action plan is yours to complete. Ticking sections off is optional, not a gate. |
| **Reorderable** | Every block in the builder — written report, figures, tables, action plan — drags up and down, and the document follows. Written report sits first by default. |
| **Locked** (`computed`) | Every figure, the summary tiles and the statistics table. They are calculated from the confirmed rows and cannot be typed over. An audit whose numbers can be hand-edited is not an audit. |

**Driving action.** Findings are useless without a change that has a name and a date on it, so
the report ends in two sections built for that:

- **Priority — fix these first.** A Pareto cut: in the sample, 3 of the 7 criteria account for
  **67%** of all missing documentation. Seven half-finished actions change nothing.
- **Action plan.** One row per priority gap: a chosen change to the record, an owner, a date,
  and how you will know it worked (pre-filled with the re-audit measure and the current
  baseline). Each gap offers 3–4 ready-made interventions, tagged by type — *Form*, *EPR*,
  *Ward routine*, *Prompt*, *Teaching* — or write your own.

**Figures** (all optional, all off unless chosen):

- **Compliance by criterion** — stacked bar, ranked worst first. The systemic gaps.
- **Completeness distribution** — histogram of criteria documented per case. Answers "a few bad
  cases, or every case?" before anyone asks.
- **Gaps that occur together** — a co-occurrence matrix. Microbiology and 48–72h review are both
  missing in the *same* 17 of 30 cases: one broken step in the workflow, not two problems, and
  one change may fix both.
- **Breakdown by subgroup** — compliance split by any field collected with the case (`Ward:`,
  `Admission:`). In the sample: Emergency Department 34% against specialty wards 90%, and out of
  hours 37% against in hours 82%. That is what turns "documentation is poor" into "fix the front
  door" — a targeted action rather than a trust-wide memo.

Charts are hand-built inline SVG with hover tooltips: no chart library, no CDN, still one file.
Colours follow the NHS identity palette. The three status colours were validated for
colour-vision deficiency (worst adjacent pair ΔE 12.7 protan, 29.5 normal vision); every
segment carries a direct label and the statistics table repeats the data, so nothing depends
on colour alone.

**Print / save as PDF** drops the tool chrome, the builder and the edit controls, and prints the
document alone.

## Staying out of medical device territory

The line that matters is *individual patient* and *clinical purpose*. AuditPilot deliberately
sits on the safe side of both, and the code enforces it rather than relying on good intentions:

- It reports whether information is **present in a record**, and aggregates that across a sample.
  That is documentation audit and service evaluation, not care of a patient.
- **Interventions come from a fixed library, not free generation.** Every entry changes how
  something is *recorded* — a form field, a system setting, a ward-round routine, a teaching
  point. None names a drug, a dose, a threshold or a clinical action. The offline engine cannot
  invent one; there is no path by which it produces a clinical recommendation.
- Nothing is generated **per patient**. Actions attach to a criterion across the whole sample.
- The clinician chooses the action, the owner and the date. The tool proposes candidates.
- An intended-purpose statement is carried in the app and at the foot of every report.

For anything beyond a prototype, the declared intended purpose is what determines
classification — get it confirmed rather than inferred.

## What's in it

- **A 30-case synthetic sample** — the sample size the audit checklist actually asks for.
  Realistic spread: indication 87%, drug/dose/route 97%, allergy 70%, duration 50%,
  guideline 63%, microbiology 40%, 48–72h review 40%. Overall 64%.
- **Two built-in templates** — antimicrobial prescribing (NICE NG15 / Start Smart Then Focus,
  7 criteria) and VTE risk assessment (NICE NG89, 6 criteria) — plus **custom criteria**:
  paste your own, one per line. Hundreds of audit standards exist, so a library is pointless;
  the engine is the same.
- **Three statuses**: Met / **Not documented** / **Not met**. "Not documented" is the honest
  answer when the record is silent — the commonest real audit finding. A cell is **Not met**
  either because the notes document a failure or because the notes contradict themselves; the
  second carries a `conflict` flag, is counted separately as *to resolve*, and the evidence
  panel says plainly that a clinician must resolve it. The tool never picks a side.
- **Evidence on every cell.** Click any chip for the verbatim source sentence. If it can't
  quote the notes, the answer is Not documented.
- **Per-row clinician sign-off**, surfaced in the scope bar and in the report's Methods section.
- **Gaps and suggested actions**, ranked by frequency across the sample. Every suggested action
  changes a form or a proforma. None changes treatment.
- **CSV export** — audit departments live in Excel.
- **Two engines.** Default is a deterministic offline rule engine: no network, no key, identical
  result every run. Switch to **Claude (`claude-opus-5`)** for free-text and custom criteria;
  the key is held in memory only and the request goes straight from the browser to the API.

## Safety and human oversight

- Documentation only — never a judgement on clinical care, never a treatment suggestion.
- Never infers. Absent from the text means Not documented, not "probably happened".
- Contradictions escalate to a human rather than resolving themselves.
- No row counts towards the report until a clinician confirms it.
- No database, no login, no `localStorage`, no persistence. Refresh and it's gone.
- All demo data is fictional.

## Files

| File | |
|---|---|
| `index.html` | AuditPilot — the submission. Self-contained. |
| `handover.html` | Earlier handover-note generator, kept for reference. |
