# PLAN — public-domain-scores

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) ·
> Lane: donated · Risk tier: **low** (with **medium**-risk rights and music-accuracy gates inside)

**One line:** Turn the world's public-domain sheet music — locked inside flat PDF and paper
scans — into living, machine-readable, openly-licensed scores (MusicXML, MIDI, MEI) that anyone
can play, transpose, study, search, and *read by ear or by touch*.

---

## Executive summary

There is an enormous body of music that humanity collectively owns: every composition whose
copyright has expired. Yet most of it is, in practice, *unusable as data*. It exists only as
images — scanned PDFs of century-old engravings on sites like IMSLP and the Internet Archive.
A student cannot slow it down, a teacher cannot transpose it for a different instrument, a blind
musician cannot render it to braille, an arranger cannot edit it, and a music-information-retrieval
researcher cannot analyse it. The notes are public domain; the *information* is trapped in pixels.

**public-domain-scores** is an open pipeline and an openly-licensed corpus that converts verified
public-domain scores into structured, semantic music encodings:

- **MusicXML** — the universal interchange format (imports into MuseScore, Finale, Sibelius,
  Dorico, music21). The canonical deliverable.
- **MIDI** — for playback, practice, and audio rendering.
- **MEI** (Music Encoding Initiative) — the scholarly/archival encoding for musicology and
  long-term preservation.

The hard part is **not** the conversion — usable open tools exist (Audiveris OMR, music21,
Verovio, MuseScore). The hard part, and the reason this is an *Elyos* project rather than a
weekend script, is **rigour**: (1) proving each score is genuinely free to redistribute as a
derivative — which is a *three-layer* rights question, not a single "is it old?" check — and
(2) proving each transcription is musically *faithful*, not plausible-looking OMR garbage. Both
are enforced as gates, recorded as provenance, and reviewed by a human with the relevant skill.

This plan delivers a thin, fully-verified slice first (M0–M1), then tooling and accessibility
derivatives (M2), then scale and partner adoption (M3). **No partner or hosting steward is yet
secured; the last-mile beneficiary is marked TO BE SECURED throughout, and `verifiedNeed` is
`false` on every task until one is committed.** The need (open, structured PD music) is real and
documented; the *committed recipient* is not yet named, and we say so honestly.

### Positioning — what this is, and what it is not

- **It is not "OMR-as-a-service."** The OMR engine is a *resource we leverage*, not the product.
  Any engine that improves — Audiveris today, a better model tomorrow — slots in behind a stable
  interface. The product is the **verified, provenance-rich, openly-licensed corpus** and the
  reproducible pipeline that produces it.
- **It is not a re-publisher of scans.** We do not host or redistribute the source images (their
  rights status is the publisher's problem); we publish *our own* clean, structured encoding of
  the underlying public-domain music, released under CC0.
- **It is not a transcription mill.** A score that is "probably fine" is worthless and possibly
  harmful (wrong notes taught to students). The unit of value is **one human-verified work**, not
  one OMR run.

---

## Problem & beneficiaries

**The problem.** Public-domain music is abundant as images and scarce as data. IMSLP alone hosts
hundreds of thousands of scans; almost none ship a clean, validated, semantic encoding. The
consequence:

- Teachers and students cannot transpose, re-instrument, slow down, or excerpt freely.
- Blind and low-vision musicians cannot get braille-music or accessible large-print, because
  those are generated *from* a digital encoding that doesn't exist.
- Arrangers, choirs, and community ensembles re-buy or re-typeset music humanity already owns.
- MIR / computational-musicology researchers lack large, clean, *provenanced* open corpora and
  fall back on tiny or licence-murky datasets.

**Who is helped (beneficiary classes).**

1. **Music learners and educators** — free, editable, transposable repertoire.
2. **Blind / low-vision musicians** — braille music (BRF) and large-print rendered from our
   encodings. (A concrete, under-served outcome that *only* a digital encoding unlocks.)
3. **Community ensembles, choirs, amateur arrangers** — editable parts for music they own.
4. **Digital-humanities and MIR researchers** — a clean, licence-clear, provenanced corpus.
5. **The open-music commons** (IMSLP, Mutopia, OpenScore, MuseScore, libraries) — durable,
   reusable encodings that strengthen the shared infrastructure.

**Verified need / partner — TO BE SECURED.** No partner organisation has yet committed to adopt,
host, or steward the corpus. Strong *candidate* stewards exist and are real, established open-music
efforts — **OpenScore / MuseScore.org** (already crowdsources PD→MusicXML→CC0 transcription),
**Mutopia Project**, **IMSLP / Petrucci Music Library**, **the Music Encoding Initiative (MEI)
community**, and **music libraries / RISM**. Outreach to these is an explicit M0 task. Until one
commits in writing, every Task JSON carries `verifiedNeed: false`, and the project's "shipped"
bar (M3) requires at least one named steward and one documented reuse.

---

## Goals and non-goals

**Goals**

- Produce **verified, faithful** structured encodings (MusicXML canonical; MIDI + MEI derived) of
  public-domain scores.
- Make rights **provable and recorded**: a three-layer rights determination (composition, edition,
  scan) per work, stored as machine-readable provenance.
- Make accuracy **provable and recorded**: a documented verification process with a measurable
  note-accuracy bar, signed off by a musically-literate reviewer.
- Ship **accessibility derivatives** (braille-ready BRF, large-print) as a first-class outcome,
  not an afterthought.
- Build a **reproducible, engine-agnostic pipeline** and an openly-licensed corpus that a partner
  steward can adopt.

**Non-goals (constraints as identity — what we refuse to do)**

- **No rights laundering.** We never publish an encoding whose three-layer rights determination is
  incomplete or "probably PD." Doubt = excluded, full stop.
- **No redistribution of source scans.** We reference sources; we do not re-host their images.
- **No unverified ("synthetic-confidence") transcriptions.** Raw OMR output never ships. Every
  published work passes human verification.
- **No modern/in-copyright works**, no "cleaned-up" copies of modern **Urtext/critical editions**,
  and no engravings still under an **edition or typographical/publication right**.
- **No performance/recording rights confusion** — we encode notation, not recordings; we make no
  claim over any performance.
- **No invented music.** Illegible or ambiguous passages are flagged with provenance, never
  guessed and silently filled.
- **No paywalled or ToS-restricted pipelines.** Tools and outputs stay open (see Stack).
- **No for-profit primary benefit.** The corpus serves the public commons; CC0/CC-BY release
  forecloses private capture.

---

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Baselines are 0 (greenfield project); targets are per-phase.

| # | Outcome metric | Baseline | Target (by M3 "shipped") |
|---|---|---|---|
| 1 | **Verified works published** (full three-layer rights + human-accuracy sign-off) | 0 | ≥ 100 works |
| 2 | **Rights-determination completeness** (published works with all 3 layers recorded) | n/a | **100%** (hard gate; non-negotiable) |
| 3 | **Provenance completeness** (assertions/files carrying source + method + rights link) | n/a | **100%** (CI-enforced) |
| 4 | **Note-level transcription accuracy** (audited, see §Quality) | n/a | ≥ 99.5% on the audited sample, no published work below 99.0% |
| 5 | **MusicXML validity** (validates against the MusicXML XSD; round-trips) | n/a | **100%** of published files |
| 6 | **Accessibility derivatives** (works with braille-ready BRF + large-print) | 0 | ≥ 25 works with verified BRF |
| 7 | **Partner / steward adoption** | 0 | ≥ 1 named steward adopts/hosts/cites |
| 8 | **Documented downstream reuse** (a real reuser: educator, ensemble, researcher, library) | 0 | ≥ 1 documented use case |
| 9 | **Corpus correction rate** (errors found post-publication / works) — *health metric* | n/a | trend tracked; < 5% of works ever require a correction |

We explicitly **do not** count "PDFs processed," "OMR runs," or "PRs merged" as success. Merged is
not delivered; **delivered = verified, rights-clear, in the hands of a beneficiary/steward.**

---

## Scope

**In scope**

- Conversion of **verified public-domain** scores into MusicXML (canonical), MIDI, and MEI.
- A **three-layer rights gate** (composition / edition / scan) with a per-jurisdiction decision
  tree and a machine-readable source allowlist.
- A **provenance model** recording source, edition (publisher/plate/date), rights basis, OMR
  engine + version, human editor, and a verification record per work.
- An **OMR-assisted, human-verified** transcription workflow (assistive automation, human in the
  loop for every published work).
- **Validation + conversion tooling** (MusicXML XSD validation, music21-based checks, format
  conversion, round-trip tests) and CI gates.
- **Accessibility derivatives**: braille-music (BRF) and large-print rendering.
- A simple **public catalog/explorer** over the corpus (no accounts, no visitor PII).
- Partner/steward outreach and adoption.

**Out of scope**

- Any in-copyright work; any modern Urtext/critical edition; any edition under a subsisting
  edition/typographical/publication right.
- **Re-hosting or redistributing source scan images.**
- Audio recordings, performances, or any rights claim over them.
- Lyrics that are **separately** in copyright (a PD melody may carry an in-copyright text/
  translation — treated as a distinct rights layer; in-copyright lyrics are excluded or stripped).
- Critical-edition scholarship (editorial decisions, figured-bass realisations, fingerings) — we
  transcribe what is on the page, flagging ambiguity, not produce a new scholarly edition.
- Fully-automated/headless mass conversion with no human verification (banned by quality bar).
- Building or hosting a streaming/playback *service* — we publish files, not run a product.

---

## Solution approach & architecture

This is a **content/data pipeline** project with supporting code. The pipeline is a linear,
auditable chain; each stage emits an artifact and a provenance record, and a later stage cannot
run until the gate before it passes.

```
[1 SELECT]      Candidate work chosen from an APPROVED source (allowlist).
   │            → provenance: source IRI, edition (publisher/plate/date), scan id
   ▼
[2 RIGHTS GATE] Three-layer determination (composition / edition / scan) recorded.
   │            ✗ any layer unproven → REJECT (logged, excluded). No exceptions.
   ▼
[3 TRANSCRIBE]  OMR-assisted draft (Audiveris) OR direct encoding (LilyPond/MuseScore)
   │            per the method policy. Engine + version recorded.
   ▼
[4 CORRECT]     Human editor corrects against the source image. Ambiguities flagged,
   │            never invented. Editor recorded.
   ▼
[5 VALIDATE]    MusicXML XSD validation + music21 structural checks + round-trip.
   │            ✗ invalid → back to [4].
   ▼
[6 VERIFY]      Independent musically-literate reviewer audits accuracy (note-level,
   │            visual + aural). ≥ accuracy bar → sign-off. Reviewer recorded.
   ▼
[7 DERIVE]      MIDI + MEI generated; accessibility derivatives (BRF braille, large-print).
   ▼
[8 PUBLISH]     CC0 (or CC-BY-SA where required) release + provenance record + catalog entry.
```

**Locked decisions (build-time commitments)**

- **Canonical format:** **MusicXML** (the interchange lingua franca). MIDI and MEI are *derived*
  from the verified MusicXML so all three stay consistent.
- **Engine-agnostic OMR seam:** the OMR step lives behind a thin adapter so the engine
  (Audiveris now) can be swapped without touching the pipeline. (Mirrors the Elyos
  "agent-neutral core, vendor logic in adapters" rule.)
- **Provenance format:** machine-readable sidecar per work (`<work>.provenance.json`, JSON
  Schema-defined), versioned with the encoding; **no encoding ships without it.**
- **Output licence:** **CC0-1.0** for transcriptions of PD works (we dedicate our encoding to the
  public domain to avoid any new edition-copyright trap). **CC-BY-SA-4.0** where we incorporate
  CC BY-SA source material (e.g. some Mutopia/Wikimedia inputs); **MIT** for the tooling code.
- **Human-in-the-loop is mandatory** for every published work (no headless mass publish).
- **Default jurisdiction stance:** evaluate against the **strictest reasonable** of {US, EU/host
  country, source country}; publish only if PD under the stance we adopt, and record which.

**Tech stack (all open-source; chosen for licence-cleanliness and longevity)**

- **OMR:** **Audiveris** (AGPL-3.0) for printed-notation OMR drafts. (Note its copyleft licence —
  used as an external tool to *produce data*; it does not link into our MIT tooling, so our code
  stays MIT and our data CC0. Recorded explicitly in the licence ledger.)
- **Toolkit / validation:** **music21** (BSD/MIT, MIT-compatible) — parse, validate, convert,
  structural checks, MIDI export. **MusicXML XSD** (W3C Music Notation CG) for schema validation.
- **Rendering / proofing:** **Verovio** (LGPL) to render MEI/MusicXML to SVG for visual diff;
  a MIDI synth (e.g. FluidSynth + an open soundfont) for aural proofing.
- **Direct encoding (where OMR is wrong tool):** **MuseScore** (GPL) / **LilyPond** (GPL) /
  **ABC** for manuscript or simple works where direct human encoding beats OMR.
- **Accessibility:** an open MusicXML→braille path (e.g. **FreeDots**/**Braille Music** family of
  tools) producing **BRF**; large-print via Verovio re-rendering at scale.
- **Project tooling:** TypeScript/ESM, pnpm (Elyos conventions) for the CLI/CI glue and catalog;
  Python invoked as a subprocess for music21/Audiveris steps behind the adapter.
- **CI:** GitHub Actions running XSD validation, provenance-completeness linter, allowlist check,
  round-trip tests, and `pnpm build && pnpm test && pnpm lint`.

**Data model (per work)**

- `work.musicxml` — canonical encoding.
- `work.mid`, `work.mei` — derived, regenerated by tooling.
- `work.brf`, `work-largeprint.*` — accessibility derivatives.
- `work.provenance.json` — source, edition metadata, three-layer rights determination + reviewer,
  transcription method + engine/version, human editor, verification record (auditor, sample,
  accuracy), licence, jurisdiction stance.
- Catalog index aggregates the provenance sidecars into a browsable, queryable list.

---

## Data, licensing & compliance

**This is the critical section. The default posture is conservative: when rights are unclear, the
work is excluded.**

**Why "is it old?" is not enough — the three rights layers.** A single score involves up to three
independent rights:

1. **The composition (the work itself).** PD when the author's copyright has expired. Rule varies
   by jurisdiction (commonly **life + 70** in US/EU; **life + 50** in Canada/IMSLP's home; the US
   also has the **published before 1929** bright line as of 2026). We record the composer's death
   year and the rule applied.
2. **The edition / engraving (the specific printed layout).** Even for a PD composition, a
   *specific modern edition* can carry its own protection: a **typographical / publication right**
   (e.g. UK CDPA §15: 25 years from publication of the typographical arrangement; EU
   **editio princeps** / critical-edition rights of up to 30/25 years) and modern **Urtext/critical
   editions** carry editorial copyright. We must identify the **edition** (publisher, plate number,
   year) and confirm *that edition* is itself out of any edition right — typically by using a
   pre-edition-right-window engraving (old plates) or an explicitly free edition.
3. **The scan / digitisation.** A faithful reproduction of a PD 2-D work generally **does not**
   create a new copyright (US: *Bridgeman v. Corel*; EU: **DSM Directive 2019 Art. 14** says
   reproductions of PD visual works are not protected). We rely on this to encode from a scan, but
   we still **do not redistribute the scan**; and we honour any contractual/database/ToS terms of
   the *host* (e.g. some libraries assert access terms regardless of copyright — those works are
   sourced only where terms permit, or excluded).

A work is publishable **only if all three layers clear.** The determination, the jurisdiction
stance, and the evidence (dates, plate numbers, source URLs) are recorded in
`work.provenance.json` and reviewed by the **License/rights reviewer** before transcription.

**Candidate sources and their licence posture (verified per item, never assumed):**

| Source | Posture | Notes |
|---|---|---|
| **IMSLP / Petrucci** | Per-item; mixed | Hosted in Canada (life+50); each scan tagged PD/CC/“non-PD in some countries.” Verify edition + per-jurisdiction tag; do **not** assume site-wide PD. |
| **Mutopia Project** | Mostly free | Typeset PD music, often CC BY-SA or PD. If CC BY-SA used → output **CC-BY-SA-4.0** + attribution. |
| **OpenScore (MuseScore)** | CC0 | Already PD→CC0 transcriptions; a candidate *partner* as much as a source. |
| **Library of Congress / NLS / Internet Archive** | Per-item; often PD | US gov / PD collections; verify per item and honour access terms. |
| **Wikimedia Commons** | Per-item; CC/PD | Check the file's own licence tag; CC BY-SA propagates share-alike. |

**Lyrics as a distinct layer.** A PD melody may carry an **in-copyright text or translation**. Text
is evaluated as its own rights layer; in-copyright lyrics are excluded or omitted from the encoding.

**Privacy / PII.** Minimal surface. Sheet music contains no personal data of living people beyond
historical author attribution (PD/biographical, non-sensitive). The **public catalog/explorer
collects no visitor PII**, requires no accounts, and sets no tracking. Contributor identities
(editors/reviewers) are recorded only as the handle they consent to publish. No analytics that
profile users.

**Attribution.** PD-derived encodings ship **CC0-1.0** (no attribution legally required) but the
provenance record always **credits the source edition and digitiser as a courtesy/scholarship
norm.** CC BY-SA inputs require attribution + share-alike on the derived file.

**Provenance is non-optional.** Every published artifact carries `work.provenance.json`; CI fails
any work missing it or any of the three rights layers. This is the project's spine.

---

## Quality, review & risk gates

**Risk tier: low overall**, with two **medium-risk gates embedded** (per good-deed-definition:
medium = "needs domain accuracy; reviewer with relevant skill"):

- **Rights gate (medium):** the three-layer determination, reviewed and signed by the
  **License/rights reviewer**. No transcription begins until the source is `approved` in the
  allowlist and the determination is recorded.
- **Music-accuracy gate (medium):** an **independent, musically-literate reviewer** (can read the
  notation) audits the transcription against the source — visually (Verovio render side-by-side
  with the scan) and aurally (MIDI playback) — and signs off only if it meets the accuracy bar.
  The reviewer must be **independent of the transcriber** (no self-grading).
- **Accessibility gate (medium, where applicable):** BRF braille output reviewed by a
  **braille-music-literate** reviewer before an accessibility derivative is published as verified.

No `high`-risk content here — there is **no medical/legal/safety advice** in sheet music, so no
credentialed expert sign-off is required. (If a project ever drifted into, e.g., "music therapy
guidance," that would re-tier to `high` and is out of scope.)

**Accuracy bar (measurable).** A published work must reach **≥ 99.0%** note-level accuracy
(pitch + duration + voicing) on the audited sample, with the corpus averaging **≥ 99.5%**.
Accuracy is measured by an independent auditor comparing a defined sample of measures against the
source; method, sample, and result are recorded in the provenance verification record.

**Definition of Shipped (project level).** A work is "delivered" when: (1) all three rights layers
are recorded and reviewer-approved; (2) MusicXML validates against the XSD and round-trips; (3)
the independent music-accuracy audit meets the bar and is signed; (4) MIDI + MEI derivatives
regenerate cleanly; (5) provenance is complete (CI green); (6) it is published under the correct
licence **and** a named steward/beneficiary has it or has committed to adopt the corpus. Merged
files with no steward are *not* shipped.

---

## Roadmap & milestones

Phased; each milestone has a measurable **exit criterion**. M0 is a thin foundation/cold-start;
later phases scale. Dependencies flow forward.

### M0 — Foundation & rights spine (cold-start)
**Goal:** make it *impossible* to publish an unverified or rights-unclear work, before any music
is transcribed.
**Exit criteria:**
- Three-layer **rights decision tree + jurisdiction stance** published.
- **Source allowlist schema** (`sources/allowlist.yml`) + policy merged; ≥ 3 candidate sources
  analysed, ≥ 1 `approved`.
- **Provenance JSON Schema** ratified (with the countable "verification record" defined).
- **Output-format decision** (MusicXML canonical; MIDI/MEI derived) + transcription-method policy
  (OMR-assisted vs direct-encode) published.
- **CI scaffold** live: MusicXML XSD validation + provenance-completeness linter + allowlist check;
  `pnpm build && pnpm test && pnpm lint` green.
- **License/rights reviewer named** and **partner/steward outreach initiated** (status logged).
  *(Hard exit: if the rights-reviewer seat is empty, M0 cannot exit — escalate per fallback.)*

### M1 — First verified slice
**Goal:** prove the full pipeline end-to-end on a handful of unambiguously-PD pieces.
**Exit criteria:**
- ≥ 5 works transcribed end-to-end (select → rights → OMR/encode → correct → validate → verify →
  derive → publish).
- **100% provenance + 100% three-layer rights** on the slice; CI green.
- Independent music-accuracy audit ≥ 99.0% per work; signed by a reviewer independent of the
  transcriber.
- MusicXML + MIDI + MEI produced and validated for each; corpus README + per-work catalog entries.

### M2 — Tooling, scale & accessibility
**Goal:** make the pipeline repeatable and add the accessibility payoff.
**Exit criteria:**
- music21-based **validation/conversion tooling** + **round-trip test** in CI.
- **Batch workflow** (still human-verified per work) producing ≥ 30 cumulative verified works.
- **Accessibility derivatives**: braille (BRF) + large-print for ≥ 25 works, BRF reviewed by a
  braille-music-literate reviewer.
- **Public catalog/explorer** (no accounts, no PII) live over the corpus; reuse metrics tracked.

### M3 — Scale & partner adoption (shipped)
**Goal:** reach a useful corpus size and put it in a steward's hands.
**Exit criteria:**
- ≥ 100 cumulative verified works; 100% provenance/rights maintained; fresh audit sample ≥ 99.5%.
- ≥ 1 **named steward** (OpenScore/Mutopia/IMSLP/MEI community/library) commits to adopt/host/cite.
- ≥ 1 **documented downstream reuse**.
- Sustainability + hosting + reviewer-rotation plan in effect.

---

## Work breakdown

The itemized, schema-mapped backlog lives in [`TASKS.md`](./TASKS.md): ~18 tasks across M0–M3,
each becoming an Elyos Task JSON (validated against `packages/schema/src/schemas.ts`) with
acceptance criteria and a per-milestone Definition of Done. Highlights: M0 rights/provenance/CI
spine (7 tasks), M1 first verified slice (4), M2 tooling + accessibility + catalog (4), M3 scale +
adoption + sustainability (3). A sized, unscheduled backlog follows.

---

## Governance, roles & stakeholders

- **Maintainer (Owner):** TBD — owns the repo, roadmap, merges, and final publish decision.
- **License/rights reviewer:** named, music-copyright-literate reviewer who approves every source
  and every three-layer determination. **Hard dependency** — no transcription without this seat.
- **Music-accuracy reviewer(s):** musically-literate reviewer(s) (can read notation) who audit
  transcriptions; rotated; must be independent of the transcriber of the work they audit.
- **Accessibility/braille reviewer:** braille-music-literate reviewer for BRF derivatives.
- **Steward / partner (last-mile owner):** **TO BE SECURED** — the org that adopts/hosts/cites the
  corpus (OpenScore, Mutopia, IMSLP, MEI community, a library). Owns long-term home.
- **Requestor:** `jdev1977` / beneficiary classes until a named partner is secured.
- **Contributors:** donated-lane humans running their own agent to transcribe/correct.

Reviewer rotation and a documented fallback (what happens if a reviewer seat is empty) are part of
the M3 sustainability task.

---

## Dependencies & integrations

- **External tools:** Audiveris (AGPL), music21 (BSD/MIT), Verovio (LGPL), MuseScore/LilyPond
  (GPL), FluidSynth + open soundfont, a MusicXML→BRF braille tool, the MusicXML XSD.
- **Source repositories:** IMSLP, Mutopia, OpenScore, Library of Congress / Internet Archive,
  Wikimedia Commons (all per-item licence-verified).
- **Elyos pieces:** the Task schema (`packages/schema`), the CLI workspace/PR flow
  (`packages/cli`), CI/governance workflows, the agent-instructions/refusal guardrails.
- **Upstream standards:** MusicXML (W3C Music Notation CG), MEI (Music Encoding Initiative),
  Standard MIDI.
- **Potential overlap:** could feed `a11y-alttext-commons` (accessibility), and shares the
  rights-gate pattern with `loc-public-domain-engine` and `revolutionary-patriots-kg`.

---

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Edition/typographical right missed → publish a still-protected modern engraving | Medium | High | Three-layer gate; identify edition (publisher/plate/year); prefer old-plate or explicitly-free editions; rights-reviewer sign-off | License reviewer |
| Jurisdiction mismatch (PD in Canada, not in US/EU) | Medium | High | "Strictest reasonable" stance recorded per work; publish only if clear under adopted stance | License reviewer |
| In-copyright lyrics/translation attached to PD melody | Medium | Medium | Treat text as its own rights layer; exclude/strip in-copyright text | License reviewer |
| OMR errors slip through → wrong notes taught | Medium | High | Mandatory independent human accuracy audit (visual+aural); ≥99% bar; no headless publish | Music-accuracy reviewer |
| Source host ToS/database terms restrict reuse despite PD | Medium | Medium | Honour host terms; allowlist records terms; exclude where terms forbid; never re-host scans | License reviewer |
| Audiveris AGPL contaminates our licence story | Low | Medium | Used as external tool producing data, not linked into MIT code; documented in licence ledger; outputs CC0 | Maintainer |
| No partner/steward secured → corpus orphaned | Medium | High | Early + sustained outreach (M0/M1/M3); persistent identifiers so corpus survives host changes; `verifiedNeed:false` until secured | Maintainer |
| Braille-music output inaccurate (misleads blind users) | Low | High | Braille-music-literate reviewer gate before any BRF marked verified | A11y reviewer |
| Reviewer-seat bottleneck stalls throughput | Medium | Medium | Reviewer rotation + documented fallback; size batches to reviewer capacity | Maintainer |
| Corpus correctness erodes silently after publish | Low | Medium | Public correction/issue path; track correction rate (metric #9); re-audit on change | Maintainer |
| Scope creep into critical-edition scholarship | Low | Medium | Non-goal: transcribe the page, flag ambiguity; no editorial decisions | Maintainer |

---

## Security & privacy

- **Threat surface is small** (static files + a no-account catalog), but: never write source ToS
  credentials, tokens, or keys into logs/provenance/commits (Elyos rule).
- **No PII collected.** No accounts, no tracking on the catalog; contributor handles published only
  with consent.
- **Abuse/misuse prevention:** the refusal guardrails apply — refuse any request to launder
  in-copyright music as "PD," to scrape a source against its ToS, or to strip attribution from
  CC BY-SA inputs. Such tasks are flagged, not performed.
- **Supply-chain:** pin tool versions (Audiveris/music21/Verovio) in provenance for reproducibility;
  validate third-party soundfonts/tools are openly licensed before use.
- **Integrity:** checksum published artifacts; provenance records the engine + version so any work
  can be reproduced and re-audited.

---

## Sustainability & maintenance

- **Persistent identifiers:** mint corpus/work identifiers under a host-independent namespace
  (e.g. w3id.org/PURL) so the corpus survives a change of host/steward (the unsecured-partner risk).
- **Stewardship:** the M3 partner becomes the long-term home; until then the maintainer holds it,
  with the repo public and CC0 so it can always be forked/adopted.
- **Outcome tracking:** the catalog tracks downloads/reuse; a public correction path tracks the
  correctness health metric; reuse case studies are recorded as evidence of "delivered."
- **Reviewer continuity:** rotation + documented fallback so a single reviewer leaving doesn't
  freeze the rights or accuracy gates.
- **Cost:** donated-lane (humans run their own agent + open tools); near-zero marginal cost beyond
  hosting static files. No funded lane planned (no metered API spend required).

---

## Open questions

1. **Which steward first?** OpenScore (already CC0, natural fit) vs Mutopia vs IMSLP vs a library /
   MEI community — needs a human decision and outreach.
2. **Jurisdiction stance:** adopt "strictest reasonable {US, EU, source}" as default, or pin to a
   single jurisdiction the steward operates in? (Affects which works qualify.)
3. **CC0 vs CC BY-SA default:** CC0 for PD transcriptions is proposed; confirm acceptable to
   candidate stewards (OpenScore=CC0; some communities prefer share-alike).
4. **Braille toolchain:** which open MusicXML→BRF tool is most reliable/maintained in 2026?
5. **Scope of accessibility:** BRF + large-print confirmed; is tactile/embossing or audio-described
   score in scope later?
6. **Edition-right windows:** finalise the per-jurisdiction edition/typographical-right table the
   rights reviewer will use (UK 25-yr, EU critical/editio princeps, US none).

---

## References

- Elyos: `CLAUDE.md`, `docs/good-deed-definition.md`, `packages/schema/src/schemas.ts`,
  `planning/ROADMAP.md` (Track 5 — Culture & heritage).
- Sibling plans (pattern reuse): `planning/projects/revolutionary-patriots-kg/{PLAN,TASKS}.md`
  (three-layer rights/provenance gate), `loc-public-domain-engine` (rights-gate engine).
- Formats/tools: MusicXML (W3C Music Notation Community Group), MEI (music-encoding.org),
  Audiveris, music21 (web.mit.edu/music21), Verovio (verovio.org), MuseScore/LilyPond.
- Open-music efforts: OpenScore (musescore.org/openscore), Mutopia Project (mutopiaproject.org),
  IMSLP / Petrucci Music Library (imslp.org).
- Rights references: *Bridgeman Art Library v. Corel*; EU **DSM Directive 2019/790 Art. 14**;
  UK **CDPA §15** (typographical arrangement, 25 yr); national term rules (life+70 / life+50 /
  pre-1929 US).

---

## Appendix A — Improvements applied

Twenty-five specific improvements made to the draft above (each already applied in the text):

1. **Three-layer rights model made explicit** (composition / edition / scan) rather than a single
   "is it PD?" check — the core correctness improvement, wired into the pipeline as gate [2].
2. **Per-jurisdiction "strictest reasonable" stance** added (US/EU/source), with the rule recorded
   per work — prevents the classic IMSLP "PD in Canada, not US" trap.
3. **Edition/typographical-right window table** referenced (UK CDPA §15, EU critical/editio
   princeps) so a PD *composition* in a still-protected *engraving* is caught.
4. **Scan-rights position grounded in case law** (*Bridgeman*, DSM Art. 14) so we can encode from
   scans confidently *and* a documented reason not to re-host them.
5. **Lyrics as a separate rights layer** called out (PD melody + in-copyright translation) — a
   commonly-missed failure mode.
6. **"No re-hosting source scans"** elevated to a non-goal and a risk mitigation (honours host ToS;
   shrinks our rights/liability surface).
7. **Audiveris AGPL contamination risk** addressed explicitly: used as an external data-producing
   tool, not linked, documented in the licence ledger; keeps code MIT / data CC0.
8. **Measurable accuracy bar** (≥99.0% per work, ≥99.5% corpus) replacing a vague "faithful."
9. **Independent-auditor rule** (reviewer ≠ transcriber) to forbid self-grading on accuracy.
10. **Visual + aural verification** specified (Verovio render vs scan; MIDI playback) — two
    independent error-detection channels.
11. **MusicXML chosen as canonical with MIDI/MEI derived from it**, guaranteeing the three formats
    stay mutually consistent instead of drifting.
12. **Engine-agnostic OMR adapter** (mirrors Elyos "vendor logic in adapters") so the OMR engine is
    a swappable resource, not the product.
13. **Provenance JSON Schema as a hard CI gate** — no encoding ships without a complete sidecar;
    the countable unit (verification record) is defined so the 100% metric is checkable.
14. **Accessibility promoted to a first-class outcome** (BRF braille + large-print) with its own
    metric (#6), gate (braille-literate reviewer), and beneficiary class — the strongest
    human-impact differentiator.
15. **Outcome metrics, not vanity metrics** — explicit rejection of "PDFs processed / OMR runs /
    PRs merged" in favour of verified works, adoption, and documented reuse.
16. **`verifiedNeed: false` everywhere + "TO BE SECURED"** honesty about the missing partner, with
    named real *candidates* and an M0 outreach task — no invented beneficiary.
17. **Persistent host-independent identifiers** (w3id.org/PURL) so the corpus survives the
    unsecured-steward risk (orphaning mitigation).
18. **"Doubt = exclude" posture** stated as identity and enforced at gate [2] — conservative by
    default.
19. **No-PII catalog** spelled out (no accounts, no tracking, consented contributor handles only).
20. **Reviewer rotation + documented fallback** and a "hard exit if rights-reviewer seat empty"
    rule, addressing the single-reviewer bottleneck risk.
21. **Risk re-tiering note**: low overall, but two embedded *medium* gates (rights, accuracy) and a
    statement that there is no `high`-risk advice content (no medical/legal/safety) — correctly
    placing the project in the risk framework.
22. **Correctness health metric (#9)** + public correction path so corpus quality is tracked *after*
    publish, not just at merge.
23. **"Transcribe the page, don't edit the work"** non-goal — keeps us out of critical-edition
    scholarship scope creep and the editorial-copyright it would create.
24. **Definition of Shipped requires a steward/beneficiary in hand** — operationalising
    "delivered, not merged" specifically for this corpus.
25. **Reproducibility via pinned tool versions + checksums in provenance** so any published work can
    be regenerated and re-audited years later.

---

## Review sign-off

**Reviewer pass (senior staff engineer + TPM), 2026-06-28.** Checked for: measurable metrics,
enforceable gates, risks with owners + mitigations, licence/PII/expert guardrails, sequencing, and
schema-valid tasks.

- **Metrics — PASS.** All nine success metrics have baselines (0/greenfield) and per-phase targets;
  two are hard gates (100% rights + 100% provenance). Vanity metrics explicitly excluded.
- **Gates — PASS.** Rights gate, accuracy gate, and accessibility gate are each defined, owned, and
  CI- or reviewer-enforced; pipeline stages cannot proceed past a failed gate.
- **Risks — PASS.** 11-row table, each with likelihood, impact, mitigation, and a named owner role;
  the top correctness risks (edition right, jurisdiction, OMR error) map to specific gates.
- **Guardrails — PASS.** Licence/provenance rigour is the spine; no PII; refusal guardrails applied
  (no rights laundering, no ToS scraping, no attribution stripping). Correctly tiered **low** with
  embedded **medium** gates; **no `high` advice content**, so no credentialed expert sign-off is
  required — and a note on what would re-tier it.
- **Sequencing — PASS.** M0 (spine) → M1 (verified slice) → M2 (tooling + a11y + catalog) →
  M3 (scale + adoption); dependencies flow forward; M0 cannot exit without the rights reviewer.
- **Schema validity — PASS.** TASKS.md maps every field to `schemas.ts`; the example Task JSON is
  schema-valid, uses an enum-legal `outputLicense` value pattern, and honestly sets
  `verifiedNeed:false`.
- **Fixes applied during review:** (a) clarified that MIDI/MEI are *regenerated* from MusicXML so a
  later correction can't desync formats; (b) added the "strictest reasonable jurisdiction" wording
  to the locked decisions, not just the rights section; (c) made the accessibility reviewer a named
  governance role, not an implicit step.

**Items needing a human decision before M1:** name the License/rights reviewer (hard M0 exit);
choose the first steward to court; confirm CC0-vs-CC-BY-SA default; finalise the edition-right
jurisdiction table. **Outstanding honest gap:** no partner/steward is secured — `verifiedNeed`
stays `false` until one commits.

**Verdict: APPROVED to begin M0.**
