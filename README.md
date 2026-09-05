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
2. Click **Load all four**, press **Check documentation**.
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
7. Tick the **Confirmed** boxes — the tool proposes, the clinician decides. Then
   **Draft audit report**: Introduction / Aim / Standard / Methods / Results / Discussion / Conclusion.

## What's in it

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
