# Competitive & Improvement Analysis — `public-domain-scores`

*Analyst pass, 2026-06-29. Grounded in web research; citations inline. Reviews PLAN.md v0.1.0 and
TASKS.md (M0–M3, ~18 tasks).*

The plan is unusually strong: it already names the three-layer rights problem, sets a measurable
accuracy bar, mandates human verification, and refuses to count vanity metrics. This analysis
therefore focuses on the few places it is *factually stale or under-specified*, and on where the
project can actually win against incumbents.

---

## 1. Correctness & completeness review of PLAN.md

**The three-layer rights model is correct and well-grounded — but two term figures are stale.**

- **Composition layer — Canada/IMSLP "life+50" is OUTDATED (the single most important correctness
  finding).** The plan repeatedly states "Canada/IMSLP life+50" (Exec summary table; §Data; risk
  table). Canada extended its term to **life + 70** effective **30 December 2022** under CUSMA, and
  the change is non-retroactive — so *no new works enter the Canadian public domain until 2043*
  ([University of Waterloo](https://uwaterloo.ca/copyright-at-waterloo/news/copyright-term-changes-life-plus-70-years);
  [Smart & Biggar](https://www.smartbiggar.ca/insights/publication/70-is-the-new-50--term-of-copyright-protection-in-canada-to-become--life-plus-70--on-december-30--2022)).
  This matters two ways: (a) IMSLP's historic "Canadian server = more PD than the US/EU" advantage
  has largely evaporated, so the plan's framing of IMSLP as a life+50 haven is wrong; (b) the
  jurisdiction decision tree (`rights-001`) must be rebuilt around life+70 for Canada, with a
  frozen-until-2043 note. **Fix the figure everywhere it appears.**
- **US bright line is one year stale.** Plan says "published before 1929 … as of 2026." On 1 Jan
  2026, 1930 works entered the US public domain, so the correct 2026 bright line is **published
  before 1931 (i.e. 1930 and earlier)**. Minor, but the rights tree must auto-roll annually — bake
  "before (currentYear − 95)" into the logic rather than a hard-coded year.

**Edition / engraving layer — accurate and the project's true differentiator-of-rigor.** UK CDPA
**§15 = 25 years** typographical-arrangement copyright from year of first publication is confirmed
([legislation.gov.uk](https://www.legislation.gov.uk/ukpga/1988/48/contents);
[Wikipedia/CDPA](https://en.wikipedia.org/wiki/Copyright,_Designs_and_Patents_Act_1988)). The EU
critical/scientific-edition neighbouring right (Term Directive 2006/116/EC Art. 5, **up to 30 yrs**;
Germany implements **25 yrs**) is confirmed
([Glasgow eprint, Margoni/Perry](http://eprints.gla.ac.uk/129331/1/129331.pdf)). One refinement: the
plan conflates two distinct EU rights — **Art. 4 editio princeps** (25 yrs for first publication of a
*previously unpublished* PD work) vs **Art. 5 critical/scientific editions** (≤30 yrs). The decision
tree should separate them, because they trigger on different facts (first-ever publication vs
scholarly re-editing). Good catch in the plan that **typographical-right caselaw means re-engraving
from scratch defeats §15** (a facsimile copy is the infringement, not a fresh typesetting) — this is
exactly why producing *new* engravings is the safe path, and the plan should state this rationale
explicitly as a feature, not just a constraint.

**Scan / digitisation layer — correct.** US *Bridgeman v. Corel* + **EU DSM Directive 2019/790
Art. 14** (faithful reproductions of PD visual works are not protected unless original) is confirmed
([EUR-Lex](https://eur-lex.europa.eu/eli/dir/2019/790/oj/eng);
[Communia](https://communia-association.org/2019/06/25/implementing-copyright-directive-protecting-public-domain-article-14/)).
The plan's "encode from scans but don't re-host them, and honour host ToS/database rights" posture is
the right conservative line. Add a note that Art. 14 is an *EU* rule unevenly transposed and the
*sui generis database right* over a collection (e.g. a library's catalog) is a separate layer from
the scan itself.

**OMR accuracy + human proofreading — realistic, with one number to soften.** Audiveris self-reports
**~80–90%** on clean printed scores, **60–75%** on moderately complex multi-staff music, and much
worse on handwritten/dense material; its own docs say "**100% recognition … is simply out of reach**"
and ship a correction GUI for exactly this reason
([audiveris.com accuracy page](https://audiveris.com/how-accurate-is-audiveris-music-recognition/);
[Audiveris handbook](https://audiveris.github.io/audiveris/_pages/handbook/)). This *validates* the
plan's core thesis (OMR is an assistive draft, never a product) but exposes a throughput risk: going
from ~85% raw OMR to the plan's **≥99.0%/≥99.5%** bar is heavy human labour, and dense piano/orchestral
scores may be *faster to direct-encode* than to correct. The method policy (`format-001`) should set
an explicit OMR-vs-direct-encode decision rule keyed to complexity, and the accuracy metric should
distinguish **per-work %** (auditable) from corpus average — already done, good.

**Transcription-error harm — handled.** Independent reviewer (≠ transcriber), visual (Verovio vs
scan) + aural (MIDI) channels, "flag ambiguity, never invent" — this is the correct standard for a
deliverable where a wrong note actively misteaches a student. One addition: define a **severity
taxonomy** (wrong pitch vs missing dynamic vs layout) so the 99% bar isn't gamed by trivial vs
critical errors weighing equally.

**Format choice — correct.** MusicXML canonical (universal interchange into MuseScore/Finale/
Sibelius/Dorico/music21), MIDI + MEI *derived* from it so they can't desync, is the right call and
matches how OpenScore/IMSLP themselves ship. Consider committing **MEI as a co-archival master**
rather than pure derivative for the scholarly-corpus spin-off, since MEI captures editorial/source
markup MusicXML loses on round-trip.

**New-engraving licensing — sound, with one risk.** CC0-1.0 for PD-derived encodings is the
mainstream norm (OpenScore, Open Goldberg Variations both CC0) and *defensively* avoids the project
itself creating a new edition-copyright trap. CC-BY-SA-4.0 where CC-BY-SA inputs (Mutopia/Wikimedia)
are incorporated is correct. The under-stated risk: **Audiveris is AGPL-3.0** — the plan addresses it
(external tool producing data, not linked) but should add that **AGPL does not reach the data output**
(output of a program is not a derivative of the program), citing the FSF position, so the CC0 data
claim is airtight in the licence ledger.

**Scope — appropriately narrow.** Excluding modern Urtext/critical editions, in-copyright lyrics as a
*separate layer*, and refusing critical-edition scholarship are all correct and rare-to-see calls.

---

## 2. Competitive landscape

| Project | What it is | Strengths | Weaknesses / gap for us |
|---|---|---|---|
| **IMSLP / Petrucci** ([imslp.org](https://imslp.org)) | ~Hundreds of thousands of PD score **scans** | Vast catalog; per-item rights tags; the de-facto source layer | **Mostly flat PDF scans, not data**; per-jurisdiction tags now complicated by Canada→life+70; almost no validated semantic encodings |
| **OpenScore (IMSLP × MuseScore)** ([openscore.cc](https://openscore.cc/blog), [musescore.org intro](https://musescore.org/en/user/57401/blog/2017/01/11/introducing-openscore)) | Crowdsourced PD→MusicXML→**CC0**; Lieder + String-Quartet corpora | Exactly our model & licence; 1,300+ Lieder w/ rich linked metadata; MIR-ready GitHub/Zenodo mirrors ([Zenodo v3](https://zenodo.org/records/15450144)) | **Lieder transcription is dormant as of 2024** ([MuseScore forum](https://musescore.org/en/node/362905)); no systematic three-layer rights provenance; no accessibility (braille) pipeline; crowd accuracy uneven |
| **MuseScore.com** ([musescore.com]) | Mass community score-sharing platform | Huge volume; great editor | **Freemium/paywalled downloads**; rights-murky user uploads; not a verified PD corpus |
| **Mutopia Project** ([mutopiaproject.org](https://www.mutopiaproject.org/), [Wikipedia](https://en.wikipedia.org/wiki/Mutopia_Project)) | Volunteer **LilyPond** typesettings of PD music | **2,124 pieces (2024)**, freely modifiable; clean source code | LilyPond-source-first (not MusicXML/MEI-first); slow volunteer cadence; thin/inconsistent provenance; no formal accuracy audit or braille |
| **CPDL / ChoralWiki** ([cpdl.org](https://www.cpdl.org/), [Wikipedia](https://en.wikipedia.org/wiki/Choral_Public_Domain_Library)) | Choral/vocal PD repository, 501(c)(3) | **~54,000 works / 5,300 composers (Nov 2025)**; many editable source files | Choral-only; quality varies; mixed formats/licences; no audited accuracy bar or structured provenance |
| **Project Gutenberg (sheet music)** | Small set of digitized PD scores | Trusted PD brand; clear process | Tiny music holdings; image/PDF-centric; not a structured-encoding effort |
| **Audiveris** ([github](https://github.com/Audiveris/audiveris)) | AGPL OMR **engine** | Best open OMR draft tool; reliable structural detection | **A tool, not a corpus**; ~85% ceiling needs human correction — our resource, not our competitor |
| **Open Goldberg Variations** ([opengoldbergvariations.org](http://opengoldbergvariations.org/), [Wikipedia](https://en.wikipedia.org/wiki/Open_Goldberg_Variations)) | One landmark CC0 score+recording (Bach BWV 988), 2012 | Proved the CC0 + public-peer-review model; high quality | **One work**; not a pipeline or scalable corpus |

Key takeaway: **the open-music commons is broad but shallow on *verified, structured, provenanced*
encodings** — IMSLP is scans, OpenScore is partly dormant and rights-light, Mutopia is small and
LilyPond-first, CPDL is choral-only. None combines all of: machine-readable + audited accuracy +
three-layer rights provenance + accessibility.

---

## 3. Gaps we can fill

1. **A rights-provenanced corpus.** No incumbent ships a per-work, machine-readable three-layer
   (composition/edition/scan) rights record. This is uniquely defensible and reusable.
2. **An audited accuracy bar.** OpenScore/Mutopia/CPDL rely on crowd review with no published
   per-work note-accuracy threshold; a signed ≥99% independent audit is a genuine differentiator.
3. **Accessibility (braille BRF + large-print/MSN) as a first-class output.** OpenScore *promised*
   braille/MSN in 2017 but it is not a delivered, reviewed pipeline anywhere. A blind musician
   literally cannot get braille without a digital encoding — this is the strongest human-impact gap.
4. **Format-agnostic master (MusicXML canonical → MIDI/MEI derived) with round-trip CI.** Mutopia is
   LilyPond-locked; we ship the interchange format plus the scholarly (MEI) one, validated.
5. **Reviving stalled momentum.** OpenScore Lieder is dormant; a disciplined pipeline that can pick
   up genres OpenScore never reached (early hymnody, folk tunes, non-Lieder vocal) fills white space.

---

## 4. Differentiators to win

1. **Rigor as the product, not volume.** vs IMSLP (scans) and crowd corpora: every work carries a
   complete, machine-checkable rights + verification record. We compete on *trust*, not count.
2. **Accessibility payoff no one else ships.** Reviewed BRF braille + large-print is a concrete,
   under-served outcome that only a clean digital encoding unlocks.
3. **Engine-agnostic, reproducible pipeline.** Pinned tool versions + checksums + provenance mean
   any work can be re-audited/regenerated years later — beyond any incumbent's reproducibility.
4. **Steward-handoff posture.** Built CC0 and host-independent (PURL/w3id) so OpenScore/Mutopia/IMSLP/
   a library can *adopt* it — we complement rather than fork the commons (turns competitors into
   beneficiaries; metric #7).
5. **Honest currency.** A rights tree that is *correct for 2026* (post-CUSMA Canada, rolling US line)
   beats incumbents still reasoning from life+50 assumptions.

---

## 5. Claude API leverage (assistive only)

**Where Claude helps (workflow, metadata, QA-assist — never the source of truth):**

1. **Metadata enrichment & catalog:** generate/normalize work titles, composer dates, instrumentation,
   incipit text, RISM/Wikidata (CC0) linkage; draft catalog entries and multilingual labels.
2. **Rights-research drafting:** assemble the *evidence dossier* for the human rights reviewer —
   composer death year, edition plate/date lookup, jurisdiction term computation, flag "this looks
   like a modern Urtext/critical edition → likely edition-right → exclude." Claude proposes; the
   License reviewer decides.
3. **OMR-QA triage (anomaly flagging):** scan MusicXML/MEI for musically implausible patterns
   (parallel-octave artifacts, impossible rhythms not summing to the meter, out-of-range notes,
   sudden voice drops) and produce a ranked "suspect measures" list to focus the human auditor —
   directly powering backlog `quality-001`. It flags; the musician confirms.
4. **Format/representation glue:** drive music21/Verovio/Audiveris subprocess orchestration, write
   conversion + round-trip test harnesses, generate provenance-sidecar scaffolding, and author the
   CI linters (XSD/provenance/allowlist).
5. **Documentation & outreach:** draft the decision-tree doc, steward outreach briefs, and reuse
   case studies.

**Where Claude must NOT decide (hard guardrails):**

- **Note accuracy** — verified only by a musically-literate human against the source (visual + aural).
  Claude must never "confirm correctness" of pitches/durations.
- **PD/edition status** — the three-layer determination is signed by the License reviewer; Claude's
  research is input, never the ruling. No "probably PD."
- **OMR error correction sign-off** — Claude may *suggest* corrections but a human edits and an
  independent human audits.
- **No fabricated notes** — illegible/ambiguous passages are flagged with provenance; Claude must
  never infill a plausible-but-invented note, dynamic, or articulation.
- **License conclusions** — final licence selection (CC0 vs CC-BY-SA, AGPL-tool implications) is human-
  ratified into the licence ledger.

Use the Anthropic Messages API for the assist tasks; model/pricing specifics should be pulled from
the `claude-api` skill before any funded run — though this project is donated-lane and plans no
metered spend, so Claude usage is the contributor's own interactive agent.

---

## 6. Ten concrete optimizations

1. **Fix the Canada term everywhere** to life+70 (post-2022 CUSMA) and add the "frozen until 2043"
   note; stop treating IMSLP as a life+50 haven.
2. **Make the US bright line self-rolling** (`year < currentYear − 95`) instead of the hard-coded
   "1929," which is already a year stale in 2026.
3. **Split the EU edition rights** in the decision tree: Art. 4 *editio princeps* (25 yr, unpublished
   PD work first published) vs Art. 5 *critical/scientific edition* (≤30 yr; DE 25 yr) — they trigger
   on different facts.
4. **Add an OMR-vs-direct-encode complexity rule** to `format-001` (e.g. dense piano/orchestral →
   direct-encode LilyPond/MuseScore; clean single-staff print → Audiveris draft), since correcting
   ~85% raw OMR on dense scores can be slower than re-typesetting.
5. **Define an error-severity taxonomy** (critical pitch/rhythm vs cosmetic) so the ≥99% bar measures
   what harms performers, not trivial layout deltas.
6. **Ship a reference "golden" work end-to-end first** (an unambiguous pre-1900 old-plate piece, e.g.
   a Bach chorale) as the pipeline's executable spec + onboarding template before scaling.
7. **Adopt MEI as co-archival master**, not pure derivative, so editorial/ambiguity/source markup
   (which MusicXML drops on round-trip) survives for the researcher spin-off.
8. **Publish the rights decision tree as machine-readable** (the same YAML the allowlist linter
   consumes) so CI can *mechanically* re-flag a work if a term rule changes — turning the Canada-style
   error into a one-line config fix, not a corpus-wide manual re-audit.
9. **Pin the braille toolchain now** (open question #4): evaluate the MusicXML→BRF options early
   (FreeDots and successors) and record reliability, because the a11y differentiator depends on it and
   M2 commits to ≥25 reviewed BRF works.
10. **Pre-negotiate the steward handoff format with OpenScore** (CC0, GitHub/Zenodo mirror, same
    metadata schema they use for Lieder) so adoption is a merge, not a migration — and so our output
    can *revive* their dormant corpus rather than compete with it.

---

## 7. Parallel & perpendicular spin-offs

- **Shared rights-gate engine with `loc-public-domain-engine` and `revolutionary-patriots-kg`.** The
  three-layer determination + machine-readable provenance + jurisdiction tree is the *same pattern*.
  Extract it as a reusable Elyos module (`rights-gate`) so music, LoC texts, and the patriots KG share
  one audited engine — multiplies the value of getting it right once.
- **`world-folktales-open` tie-in.** PD folk *tunes* (melody + the folktale text) — a natural joint
  corpus: our engraved melody + their narrative, both CC0, cross-linked. Backlog `data-004`
  (folk-tune collections) is the bridge.
- **`read-aloud-audio` / audio rendering.** Our verified MIDI + an open soundfont (FluidSynth) yields
  CC0 reference audio and practice tracks; pairs with read-aloud for "score + narrated walkthrough"
  learning material, and seeds audio-described-score a11y (`a11y-002`).
- **An open engraved-score corpus as MIR infrastructure.** Like OpenScore's Lieder/String-Quartet
  corpora feeding ISMIR research, a clean licence-clear corpus is itself a research good — pursue a
  Zenodo DOI release per milestone for citability.
- **An MCP server over the corpus.** Expose incipit/melodic search (backlog `search-001`),
  transposition, and part-extraction as MCP tools so any agent can query "give me a CC0 SATB setting
  of X transposed down a third as MusicXML" — turning the corpus into queryable infrastructure and a
  flagship for the broader Elyos MCP story.
- **`a11y-alttext-commons` cross-feed.** Braille/large-print rendering expertise and reviewers overlap
  with the accessibility-alt-text effort; share the a11y reviewer pool and tooling.

---

## 8. Open questions

1. **Which steward, and does OpenScore want a hand-up?** OpenScore Lieder is dormant — is the best move
   to *revive* it (our pipeline feeding their CC0 corpus) rather than stand alone? Confirm appetite.
2. **OMR-vs-direct-encode economics:** at what complexity does correcting Audiveris output cost more
   than re-typesetting? Needs a measured pilot to set the `format-001` rule (and throughput estimates).
3. **Braille toolchain reliability in 2026** (plan's own Q4): which MusicXML→BRF tool is maintained and
   accurate enough to stake the a11y differentiator on?
4. **Jurisdiction stance vs steward jurisdiction:** "strictest reasonable {US,EU,source}" maximizes
   safety but shrinks the corpus; if the steward is US-only, a US-pinned stance admits far more works.
   Decide before M1 (affects which works qualify).
5. **Database / sui generis rights** of source collections (separate from scan copyright) — does the
   allowlist need a per-source database-right field, esp. for EU library sources?
6. **Severity-weighted accuracy:** should the ≥99% bar be note-count or severity-weighted, and who
   defines the taxonomy (the music-accuracy reviewer role)?
7. **CC0 vs CC-BY-SA default friction:** if a CC-BY-SA Mutopia input is ever mixed into a CC0 work, the
   whole derivative goes share-alike — does the pipeline forbid mixing licences within one work?
