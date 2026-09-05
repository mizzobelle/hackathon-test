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

For a public link (the hackathon submission), any static host works — GitHub Pages, Netlify
drop, Vercel. It is one file.

## The 3-minute demo

1. **Before you start** — the checklist at the top. Signals we know an audit is a methodology,
   not a text box. Collapse it.
2. Click **Load 30-case sample**, press **Check documentation**. Thirty patients against seven
   criteria — 210 documentation checks — in about a second. (**Load all four** gives the short
   version if you are tight on time.)
3. **Case 1 (7/7)** — the golden path. Click a green cell: the source sentence is quoted.
4. **Case 2 (1/7)** — the wall of amber. Click one: *"No relevant documentation found."*
   This is the most common real audit finding — the care probably happened, the notes can't prove it.
5. **Case 4** — the failure case. Two purple **Needs review** cells:
   - *Allergy status*: the clerking says NKDA, the drug chart says penicillin allergy.
     The tool refuses to pick a side and hands it to a human.
   - *Microbiology*: a urine sample is documented, but not whether it was sent before the
     first dose. It will not assume.
6. **Read the table vertically.** The bottom row shows column compliance. Two criteria fail
   in almost every case — that's the audit finding. Reading notes one at a time you never see it.
7. **Confirm all rows** — the tool proposes, the clinician decides; nothing reaches the report
   unconfirmed. Then **Draft audit report**.

## The report

**Draft audit report** builds a document, not a text box:

- **Four summary tiles** — documented compliance (64%), checks Not documented, fully compliant
  cases, and checks escalated to a clinician.
- **Figure 1 — documentation status by criterion.** A stacked bar per criterion, ranked worst
  first, showing Met / Not documented / Needs review / Not met across all 30 cases. This is the
  chart that carries the pitch: *microbiology* and *48–72h review* are documented in 12 of 30
  cases each. That is a form problem, not a practice problem, and you cannot see it reading
  notes one at a time.
- **Figure 2 — distribution of completeness.** A histogram of how many criteria each case
  documented. The 30-case sample is bimodal — well-documented cases and near-empty ones — which
  answers "is this a few bad cases or systemic?" before anyone asks.
- **Descriptive statistics table** — counts and % Met per criterion, with totals, median, mean
  and range.
- **The narrative report** in the standard seven headings, with the descriptive statistics
  written into Results and the weakest criteria named in Discussion.
- **Print / save as PDF** — a print stylesheet drops the tool chrome and prints the document alone.

Charts are hand-built inline SVG with hover tooltips: no chart library, no CDN, still one file.
The four status colours were validated for colour-vision deficiency (worst adjacent pair
ΔE 16.2 protan, 30.3 normal vision); every segment carries a direct label and the statistics
table repeats the data, so nothing depends on colour alone.

## What's in it

- **A 30-case synthetic sample** — the sample size the audit checklist actually asks for.
  Realistic spread: indication 87%, drug/dose/route 97%, allergy 70%, duration 50%,
  guideline 63%, microbiology 40%, 48–72h review 40%. Overall 64%.
- **Two built-in templates** — antimicrobial prescribing (NICE NG15 / Start Smart Then Focus,
  7 criteria) and VTE risk assessment (NICE NG89, 6 criteria) — plus **custom criteria**:
  paste your own, one per line. Hundreds of audit standards exist, so a library is pointless;
  the engine is the same.
- **Four statuses**, not two: Met / Not met / **Not documented** / **Needs review**.
  "Not documented" is the honest answer when the record is silent. "Needs review" is what the
  tool returns instead of guessing when the record contradicts itself.
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
