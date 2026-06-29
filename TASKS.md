# TASKS — public-domain-scores

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

Itemized backlog for transcribing **verified public-domain** sheet music into open, structured
formats (MusicXML canonical; MIDI + MEI derived) with full provenance and accessibility
derivatives. See [`PLAN.md`](./PLAN.md) for positioning, the three-layer rights gate, the accuracy
bar, and the roadmap (M0–M3).

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against `packages/schema/src/schemas.ts`.
Field mapping:

- **id** — stable `public-domain-scores-<area>-NNN` (the table ID).
- **title** — the table Title.
- **project** — `public-domain-scores`.
- **type** — one of `code | research | writing | data | design-spec | maintenance` (table "Type").
- **lane** — `donated` for all tasks here (humans run their own agent + open tools; near-zero
  marginal cost). Any future metered run would be `funded` and must add `fundedBudgetUsd`.
- **priority** — `high | medium | low`.
- **domain** — e.g. `["open-music","public-domain","culture-heritage","education","accessibility"]`.
- **riskTier** — `low | medium | high`. **Rights** and **music-accuracy** and **braille** tasks are
  **medium** (need a reviewer with the relevant skill); scaffolding/docs are **low**. No `high`
  tasks (no medical/legal/safety advice in sheet music).
- **urgent** — boolean (default `false`).
- **deliverable** — `pr | dataset | document | translation` (table "Deliverable"). Score corpora are
  `dataset`; tooling/CI/catalog are `pr`; specs/policies/audits are `document`.
- **tokenEstimate** — `small | medium | large` (table "Size").
- **status** — `open | in-progress | review | delivered | done` (start `open`).
- **context / objective / acceptanceCriteria[] / resources[] / output** — per task.
- **requestor** — `jdev1977` / beneficiary class until a named partner is secured.
- **verifiedNeed** — **`false`** on every task while no partner/steward is committed (honest: the
  *need* for open structured PD music is real, but the last-mile beneficiary is **TO BE SECURED**).
- **outputLicense** — `MIT` (tooling code), `CC0-1.0` (PD-derived encodings/datasets/docs), or
  `CC-BY-SA-4.0` (where CC BY-SA source material is incorporated, e.g. some Mutopia/Wikimedia inputs).

> **Standing guardrail on every transcription/data task:** no source may be touched until its
> `sources/allowlist.yml` entry is `approved` by the License/rights reviewer **and** the
> three-layer rights determination (composition / edition / scan) is recorded. Any task proposing to
> publish an unverified work, re-host a source scan, launder an in-copyright edition as "PD," scrape
> a source against its ToS, or strip CC BY-SA attribution is **refused and flagged** — out of scope.

---

## Milestone M0 — Foundation & rights spine (cold-start)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| public-domain-scores-rights-001 | Three-layer rights decision tree + jurisdiction stance | design-spec | medium | medium | document | — | License reviewer |
| public-domain-scores-license-001 | Source allowlist schema + analyze ≥3 sources (≥1 approved) | research | medium | medium | document | public-domain-scores-rights-001 | License reviewer |
| public-domain-scores-prov-001 | Provenance JSON Schema (rights + method + verification record) | design-spec | small | low | document | public-domain-scores-rights-001 | Maintainer |
| public-domain-scores-format-001 | Output-format + transcription-method policy (MusicXML canonical) | design-spec | small | low | document | — | Maintainer + music reviewer |
| public-domain-scores-ci-001 | CI scaffold: MusicXML XSD + provenance + allowlist linters | code | medium | low | pr | public-domain-scores-prov-001, public-domain-scores-format-001 | Maintainer |
| public-domain-scores-id-001 | Commit host-independent persistent identifier namespace | design-spec | small | low | document | public-domain-scores-prov-001 | Maintainer |
| public-domain-scores-partner-001 | Steward outreach (OpenScore/Mutopia/IMSLP/MEI/library) | research | small | low | document | — | Maintainer |

**Acceptance criteria (key M0 tasks)**

- **public-domain-scores-rights-001**
  - Documents the three independent layers — composition, edition/typographical, scan/digitisation —
    each with the test applied and evidence required.
  - Includes a per-jurisdiction term table (US pre-1929 + life+70; EU life+70 + critical/editio
    princeps; Canada/IMSLP life+50; UK CDPA §15 25-yr typographical) and adopts the
    "strictest reasonable {US, EU, source}" default stance, recorded per work.
  - Lyrics/translation handled as a distinct rights layer (in-copyright text excluded/stripped).
  - Defines the "doubt = exclude" rule and the rights-reviewer sign-off step before transcription.
- **public-domain-scores-license-001**
  - `sources/allowlist.yml` schema captures: source, edition (publisher/plate/year), URL, format,
    per-layer rights basis, host ToS/access terms, jurisdiction, and `status: approved|rejected|pending`.
  - ≥3 candidate sources analysed; ≥1 `approved`; at least one approval is an unambiguous PD case
    (e.g. a pre-1929, old-plate engraving).
  - Records any CC BY-SA source as requiring share-alike + attribution on derived files.
- **public-domain-scores-prov-001**
  - `work.provenance.json` schema defines source, edition, three-layer rights determination +
    reviewer, transcription method + engine/version, human editor, and the **verification record**
    (auditor, sample, accuracy %) — the countable unit the 100%-provenance CI gate checks.
  - No encoding may exist in the repo without a complete sidecar (linter-enforceable).
- **public-domain-scores-ci-001**
  - CI fails any `.musicxml` that does not validate against the MusicXML XSD or does not round-trip.
  - CI fails any work missing its provenance sidecar or any of the three rights layers.
  - CI rejects any work referencing a source not `approved` in the allowlist.
  - `pnpm build && pnpm test && pnpm lint` green.

**M0 Definition of Done:** three-layer rights decision tree + jurisdiction stance published;
allowlist schema + policy merged with ≥3 sources analysed and ≥1 approved; provenance schema
ratified with the verification record defined; output-format + method policy published; CI
XSD+provenance+allowlist gates live and green; persistent-identifier namespace committed; steward
outreach initiated with status logged; **License/rights reviewer named (hard exit — if the seat is
empty, M0 cannot exit; escalate per PLAN.md fallback).**

---

## Milestone M1 — First verified slice

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| public-domain-scores-pipe-001 | OMR-assisted transcription pipeline (engine adapter) | code | large | low | pr | public-domain-scores-ci-001 | Maintainer |
| public-domain-scores-data-001 | Transcribe + verify first 5 PD works → MusicXML/MIDI/MEI | data | large | medium | dataset | public-domain-scores-pipe-001, public-domain-scores-license-001 | Music-accuracy reviewer |
| public-domain-scores-qa-001 | Independent accuracy audit of M1 slice (≥99% per work) | research | medium | medium | document | public-domain-scores-data-001 | Music-accuracy reviewer |
| public-domain-scores-partner-002 | Engage ≥1 candidate steward for adoption | research | small | low | document | public-domain-scores-partner-001 | Maintainer |

**Acceptance criteria (key M1 tasks)**

- **public-domain-scores-pipe-001**
  - OMR engine (Audiveris) sits behind a thin adapter; engine + version recorded into provenance;
    swapping engines does not change the pipeline contract.
  - Implements the method policy: OMR-assisted for printed notation, direct-encode
    (MuseScore/LilyPond) where OMR is the wrong tool (manuscript, very simple works).
  - Regenerates MIDI + MEI *from* the verified MusicXML so the three formats cannot desync.
  - Emits Verovio SVG render + MIDI playback artifacts to support visual+aural proofing.
- **public-domain-scores-data-001**
  - 5 works, each from an `approved` source with a complete three-layer rights determination.
  - 100% of works carry a complete provenance sidecar; CI (XSD + provenance + allowlist) green.
  - Ambiguous/illegible passages are flagged with provenance, never invented.
  - In-copyright lyrics/translations excluded; only PD text encoded.
  - MusicXML validates + round-trips; MIDI + MEI regenerate cleanly.
- **public-domain-scores-qa-001**
  - A defined measure-level sample per work is audited against the source by a reviewer
    **independent of the transcriber** (no self-grading), visually (Verovio vs scan) and aurally (MIDI).
  - Each work ≥99.0% note-level accuracy (pitch + duration + voicing); errors filed and fixed
    before sign-off; the audit method, sample, and result are written into the provenance record.

**M1 Definition of Done:** end-to-end pipeline proven on ≥5 PD works; 100% provenance + 100%
three-layer rights; independent accuracy audit ≥99% per work signed off; MusicXML+MIDI+MEI produced
and validated under the persistent-identifier namespace; ≥1 candidate steward in active conversation.

---

## Milestone M2 — Tooling, scale & accessibility

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| public-domain-scores-tool-001 | music21 validation/conversion tooling + round-trip tests | code | medium | low | pr | public-domain-scores-pipe-001 | Maintainer |
| public-domain-scores-a11y-001 | Braille (BRF) + large-print derivatives for ≥25 works | data | large | medium | dataset | public-domain-scores-data-001, public-domain-scores-tool-001 | Braille reviewer |
| public-domain-scores-data-002 | Scale to ≥30 cumulative verified works (batch, human-verified) | data | large | medium | dataset | public-domain-scores-qa-001 | Music-accuracy reviewer |
| public-domain-scores-catalog-001 | Public catalog/explorer (no accounts, no PII) + reuse metrics | code | medium | low | pr | public-domain-scores-tool-001 | Maintainer |

**Acceptance criteria (key M2 tasks)**

- **public-domain-scores-a11y-001**
  - ≥25 verified works rendered to braille music (BRF) and large-print.
  - Every BRF reviewed by a **braille-music-literate** reviewer before being marked verified.
  - The accessibility derivative records its source MusicXML version + tool version in provenance;
    regenerating from a corrected MusicXML re-runs the braille review.
- **public-domain-scores-catalog-001**
  - Static, no-account explorer over the corpus; collects **no visitor PII**; no tracking.
  - Each work shows title, composer, source edition, rights determination, licence, and links to
    MusicXML/MIDI/MEI/BRF downloads.
  - Reuse metrics (downloads/queries) tracked; a public correction/issue path is linked.

**M2 Definition of Done:** music21 tooling + round-trip tests in CI; ≥30 cumulative verified works;
≥25 works with reviewer-approved BRF + large-print; public no-PII catalog live with reuse metrics
and a correction path; 100% provenance/rights maintained.

---

## Milestone M3 — Scale & partner adoption (shipped)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| public-domain-scores-data-003 | Scale to ≥100 verified works; fresh audit sample ≥99.5% | data | large | medium | dataset | public-domain-scores-data-002 | Music-accuracy + license reviewers |
| public-domain-scores-partner-003 | Secure steward adoption + ≥1 documented reuse | research | medium | low | document | public-domain-scores-partner-002 | Maintainer |
| public-domain-scores-sustain-001 | Sustainability, hosting + reviewer-rotation/fallback plan | writing | small | low | document | public-domain-scores-partner-003 | Maintainer |

**Acceptance criteria (key M3 tasks)**

- **public-domain-scores-data-003**
  - ≥100 cumulative verified works across ≥3 approved sources; each passed the rights gate before
    transcription; 100% provenance maintained.
  - A fresh, independent audit sample across the corpus verifies ≥99.5%.
- **public-domain-scores-partner-003**
  - A named steward (OpenScore/Mutopia/IMSLP/MEI community/library) commits to adopt/host/cite.
  - ≥1 concrete downstream reuse documented (educator, ensemble, blind musician, researcher, or
    library), with `verifiedNeed` re-evaluated to `true` for affected tasks once the partner commits.

**M3 Definition of Done (project "shipped"):** ≥100 verified works; 100% provenance/three-layer
rights; corpus audit ≥99.5%; ≥1 named steward has adopted/hosted/cited the corpus; ≥1 documented
reuse; persistence + reviewer-rotation/fallback plan in effect.

---

## Backlog / future (sized, unscheduled)

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| public-domain-scores-data-004 | Genre/era collections (e.g. early hymnody, folk tunes) | data | large | medium | dataset | Each item rights-gated; old-plate editions |
| public-domain-scores-i18n-001 | Multilingual catalog labels + composer metadata (Wikidata CC0) | data | small | low | dataset | CC0 labels only |
| public-domain-scores-search-001 | Melodic/incipit search over the corpus (MIR) | code | large | low | pr | Enables researcher reuse |
| public-domain-scores-quality-001 | Automated anomaly flagging (suspect OMR) for review | code | medium | low | pr | Assistive QA, human-confirmed |
| public-domain-scores-a11y-002 | Audio-described / tactile-embossing-ready derivatives | data | medium | medium | dataset | Pending a11y scope decision |
| public-domain-scores-parts-001 | Auto-extract instrument parts from full scores | code | medium | low | pr | Editable parts for ensembles |
| public-domain-scores-mei-001 | Enrich MEI with editorial/ambiguity markup (sourced) | data | medium | low | dataset | Flag ambiguity, no editorial decisions |

---

## Example task JSON

Schema-valid Task JSON for the first M0 task (`public-domain-scores-rights-001`):

```json
{
  "id": "public-domain-scores-rights-001",
  "title": "Three-layer rights decision tree + jurisdiction stance",
  "project": "public-domain-scores",
  "type": "design-spec",
  "lane": "donated",
  "priority": "high",
  "domain": ["open-music", "public-domain", "culture-heritage", "copyright", "accessibility"],
  "riskTier": "medium",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "public-domain-scores transcribes verified public-domain sheet music into open structured formats (MusicXML canonical; MIDI/MEI derived). A single score carries up to three independent rights: the composition, the specific edition/typographical arrangement, and the scan/digitisation. Treating 'is it old?' as the only test would publish still-protected modern engravings or in-copyright lyrics. No work may be transcribed until rights are proven and recorded. No partner/steward is yet secured. See PLAN.md (Data, licensing & compliance).",
  "objective": "Produce the rights decision tree and jurisdiction stance the License/rights reviewer applies to every work: the three layers (composition, edition/typographical, scan), the per-jurisdiction term tests (US pre-1929 + life+70; EU life+70 + critical/editio-princeps rights; Canada life+50; UK CDPA s.15 25-year typographical right), the 'strictest reasonable {US, EU, source}' default stance, the lyrics/translation-as-separate-layer rule, and the 'doubt = exclude' posture with a recorded reviewer sign-off step.",
  "acceptanceCriteria": [
    "All three rights layers documented with the test applied and the evidence each requires.",
    "Per-jurisdiction term table included (US pre-1929/life+70, EU life+70 + critical/editio princeps, Canada life+50, UK CDPA s.15 typographical 25yr).",
    "Default 'strictest reasonable {US, EU, source}' stance adopted and recorded per work.",
    "In-copyright lyrics/translations handled as a distinct rights layer (excluded or stripped).",
    "Scan-rights position grounded in Bridgeman v. Corel and EU DSM Directive Art. 14, with a no-re-hosting-of-source-scans rule.",
    "Defines the rights-reviewer sign-off step that must complete before any transcription begins, and the 'doubt = exclude' rule.",
    "Output is machine-actionable: each layer maps to a field in the provenance schema (public-domain-scores-prov-001)."
  ],
  "resources": [
    "planning/projects/public-domain-scores/PLAN.md",
    "packages/schema/src/schemas.ts",
    "docs/good-deed-definition.md",
    "https://www.w3.org/2021/06/musicxml40/",
    "https://imslp.org/wiki/IMSLP:Copyright_Made_Simple"
  ],
  "output": "A rights-determination specification document (docs/rights-policy.md) defining the three-layer decision tree, the per-jurisdiction term table, the jurisdiction stance, the lyrics layer rule, and the reviewer sign-off gate, committed via PR.",
  "requestor": "jdev1977",
  "verifiedNeed": false,
  "outputLicense": "CC0-1.0"
}
```
