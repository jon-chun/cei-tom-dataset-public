# Revision Report — `papers/cei2026/cei2026_cei-tom_dataset.tex` (+ `.bib`)

**Date:** 2026-08-20 (report slug 20260818)
**Method:** Six parallel audit agents following `workflow_ai-gen-detect-human-bib-check_v2_all_20260814.md` — bibliography verification vs. CrossRef/Semantic Scholar/OpenAlex/arXiv/ACL Anthology, inline claim-support verification against the actual sources, academically-calibrated `ai-check` style audits on three section ranges, and a mechanical/consistency audit that recomputed statistics directly from `data/human-gold/*.csv` and did a clean sandbox build.
**Priority order enforced (workflow §3):** factual/citation correctness → claim preservation → voice → style. Detector scores are a review instrument, never a loss function.

---

## 1. Overview

The paper builds cleanly (38 pp, 0 errors, 0 undefined refs/citations, citations balanced 70↔70 in both directions) and its empirical core is largely verifiable: the audit reproduced the agreement counts, confusion pairs, adjacency rates, valence-coherence numbers, and every table mean directly from the released CSVs. That is the good news, and it is substantial.

The bad news falls into three tiers, in exactly the order the workflow predicts for AI-assisted drafts:

1. **Citation substance (worst).** Of 15 spot-checked inline claims, **7 are contradicted by their sources** (Kosinski 95%→actual 75%; GoEmotions "α=0.46" is actually a BERT F1, not agreement; SemEval-2007 "κ≈0.28" — the task reported Pearson r, never κ; EmoBank correlations wrong in value and meaning; Sap et al. "55–65% with CoT" — actually 55%/60% with few-shot, no CoT; Bostan & Klinger never discuss agreement/κ ranges; NRC "~27K"→~14K). Two bib entries carry **fabricated author lists** (`hyun2023diplomat`, `liu2024emobench`), two have bad DOIs, and the PUB benchmark is compared in Table 1 and prose with **no citation at all**. The §5.5 benchmark-comparison paragraph (L393) is built on three of the contradicted numbers and needs substantive rework, not polish.
2. **Internal consistency.** An arithmetically impossible adjudication count (47 flagged < 94 splits, < 72 documented notes) propagated to a figure and the datasheet; the headline human baseline (54%) is a proportion-of-scenarios, not an accuracy, so the human–model gap is mis-stated everywhere it appears; "majority agreement" is used with three incompatible definitions; one per-subtype accuracy (60.9%) is wrong due to four malformed cells in the released CSVs; Appendix F's "sample records from the released CSVs" **are not in the CSVs**; "five worked examples" precedes three.
3. **Humanization (least severe, real but fixable).** Calibrated ai-check scores by range: front 14/27, middle 10/27, back 13/27 — "mixed/uncertain, AI-assisted interpretive layer over a human empirical core." The dominant tells are structural, not lexical: whole sentences repeated near-verbatim across sections (the "dissociation → qualitatively different processing failures" sentence 3×; the VAD-consistency paragraph 2×; the impact lists 2×), templated Related-Work positioning closers ("CEI differs from…" 2×), a repeated double-em-dash list-wrap mold (4×), and a small family of catalog vocabulary ("the landscape of", "Notably", vague "growing body of work" lead-ins).

**64 revision tasks** are enumerated below: 21 citation/claim (C), 12 numeric-consistency (N), 10 hygiene/mechanics (H), 21 style/humanization (S, several bundling multiple instances). Metrics: CRITICALITY = risk to correctness/acceptance if unfixed; DIFFICULTY = effort plus drift risk of the edit. Tasks needing an author decision (they move headline numbers or claims) are marked **[AUTHOR]**.

---

## 2. Ranked task list

### Tier 1 — HIGH criticality (fix before anything else)

| # | Loc (tex line) | Task | Diff |
|---|---|---|---|
| C1 | 92 | Kosinski: 95% → 75% (PNAS version) | low |
| C2 | 96, 393 | GoEmotions "Krippendorff's α=0.46" is a BERT macro-F1, not agreement — reword both sites | middle |
| C3 | 96, 393 | SemEval-2007 "κ≈0.28" — source used Pearson r (0.36–0.68), 1,250 headlines | middle |
| C8 | 393 | EmoBench κ=0.852 mischaracterized ("binary yes/no, decontextualized" — actually MCQ with designed-correct answers, contextualized) | middle |
| C5 | 92, 499 | Sap et al.: "55–65% … with chain-of-thought" → 55% (SocialIQa) / 60% (ToMi) with few-shot; no CoT in source | low |
| C4 | 96 | EmoBank "0.64–0.72 between the two perspectives" → 0.61/0.63 inter-annotator, per perspective | low |
| C6 | 96 | Bostan & Klinger: attributed agreement/κ-range claims not in the survey | low |
| C9 | bib | `liu2024emobench` fabricated authors → Sabour et al., add DOI, rename key | low |
| C10 | bib | `hyun2023diplomat` fabricated authors → Li, Zhu, Zheng | low |
| C11 | 121, 135, bib | PUB compared but never cited — add Sravanthi et al. (Findings ACL 2024) entry + `\citep` | low |
| C12 | 135 | DiPlomat does NOT target "diplomatic settings" (Di-alogue Prag-mat) — contradicts own Table 1 | low |
| N1 | 259, 283, 678 | Adjudication count 47/15.7% arithmetically impossible (94 splits, 72 notes) → 72/24.0% **[AUTHOR]** | low |
| N2 | 78, 499, 571, 573, 624 | 54% "human majority agreement" is a scenario proportion, not accuracy — comparable figure is 60.9%; gap ≈36 pts **[AUTHOR]** | middle |
| N3 | 351, 355, 599 | "Majority agreement" used with 3 incompatible definitions — one term per metric | low |
| N4 | 355 | Mixed-signals 60.9% → 58.9%; inconsistent denominators across subtypes; "mean 60.9%, N=15" is subtype-mean not annotator-mean (60.4%) **[AUTHOR]** | middle |
| N5 | data CSVs | 4 malformed gold cells (ids 171/189 empty; 193 `anticipationa`; 268 `surpise`) — dataset has 298 usable golds, not 300 **[AUTHOR]** | low |
| N6 | 897–912 | Appendix F "sample records from the released CSVs" are not in the CSVs — replace with verified ids 1/18/633 or relabel as illustrative | low |
| H1 | 21, 22, 24, 51 | TBD placeholders + TODO reach the rendered PDF header | low |
| H2 | repo | Venue mismatch: README says NeurIPS 2026 D&B; paper is DMLR (`dmlr2e.sty`, `\editor`, `\impact`); zip says dmlr2026 **[AUTHOR]** | middle |
| S1 | 521, 573, 624 | "dissociation … qualitatively different processing failures" sentence repeated 3× near-verbatim | middle |
| S2 | 428, 562 | VAD-consistency paragraph (same numbers + conclusion) duplicated | middle |
| S3 | 96, 110 | "CEI differs from…" positioning frame duplicated verbatim across subsections | low |
| S4 | 56 | Double em-dash wrap in the abstract's opening sentence | low |
| S5 | 172, 176, 391 | Double em-dash list-wrap mold repeated 3× | low |

### Tier 2 — MIDDLE criticality

| # | Loc | Task | Diff |
|---|---|---|---|
| C7 | 108, 437 | NRC EmoLex "~27K terms" → ~14K words (~24K word–sense pairs) | low |
| C13 | bib | `niu2024rethinking`: DOI 404 → 10.1109/TAFFC.2025.3584775, year 2025 | low |
| C14 | bib | `danescu2013computational`: DOI doesn't exist — remove, use ACL Anthology URL | low |
| C16 | 88, 499 | Hu et al.: "conventional vs non-conventional implicatures" → "social expectation violations" (paper's actual framing) | low |
| C17 | Table 1 | "Pragmatic subtypes: 14 types" (PUB) / "2 types" (DiPlomat) count tasks, not subtypes → "4 phenomena / 14 tasks", "2 tasks" | low |
| N7 | 221 | "We present five scenarios—one per pragmatic subtype" but three follow, organized by agreement level | low |
| N8 | 571 | "30-percentage-point gap" vs 29 elsewhere (moot if N2 lands) | low |
| H4 | 236, 249 | `\S\ref{sec:annotation}` self-reference → add `\label{sec:qc}` to §4.2 and retarget | low |
| S6 | 428, 575, 626 | "VAD as complementary continuous target" stated 3× — keep fullest (575), trim others | low |
| S7/S8 | 605 vs 630/632 | Broader-Impact beneficial-uses and misuse lists duplicated wholesale in `\impact` — cross-reference, keep only new items | middle |
| S9 | 473, 499, 546 | "not an artifact of X" negative-parallelism template 3× in ~70 lines — vary one | low |
| S10 | 236, 238 | Free-text explanation requirement defined twice in adjacent sentences | low |
| S11 | 238, 322 | Within-subtype design rationale restated (322 already cross-references it) | low |
| S12/S13 | 88, 104 | Vague-attribution lead-ins ("Recent work has begun…", "A growing body of work…") ×2 | low |
| S14 | 94 | "the landscape of" (catalog vocabulary, subsection opener) | low |
| S15 | 100 | "Notably," + "highlighting the gap…" appended -ing clause | low |
| S16/S17 | 90 | Negative parallelism + templated "CEI complements them by…" closer | middle |
| S18 | 573 | Scare-quoted coinage "diagnostic sweet spot" | low |
| S19 | 389 | Bald-assertion closer "Random disagreement would not produce this pattern." | middle |
| S20/S21 | 479, 499 | Caption restates prose conclusion; 3 stacked -ing interpretive clauses in one paragraph | low |

### Tier 3 — LOW criticality (batch or skip)

C15 (SARC 1M→1.3M, L100) · C18 (Krippendorff paraphrase is load-bearing for κ=0.21 — soften or accept reviewer risk, L104) · C19 (optional key renames: `mohammad2012semeval`→`mohammad2013crowdsourcing`, `liang2022holistic`→2023, `niu2024`→2025) · C20 (Label Studio → Tkachenko et al., optional) · C21 (OV-MER wording sharpening, optional) · N9 (OOV caption 46 vs 44 reconciliation, L441) · N10 (timing: "roughly one hour"→"50–70 minutes" L238; "median"→"mean ≈1 min" L923) · N11 (one clause noting anger↔surprise is non-adjacent, L391) · N12 (label the two different "12-point" quantities, L573) · H3 (delete byte-identical `figures copy/`, 909 KB) · H5 (hardcoded "see footnote~1", L617 — label+ref) · H7 (5 overfull hboxes, worst 12 pt at L355/L906/L912) · H8 (signpost Appendix F and H from body) · H9 (caption-package unknown-class warning; broken `Hfootnote.1` hyperref anchor) · H10 (fig5 PNG is 743 KB of a 956 KB PDF) · S22–S32 (em-dash pivots L78/88/96; "underscoring the genuine ambiguity" L225; -ing tail L397; "We [verb]" opener drumbeat; "rather than" cluster; "has been studied extensively" ×2 L68/98; "provides a comprehensive" ×2 L68/90; "genuinely difficult…meaningful gaps" L571; "measures something real" L575; "genuine finding" L599; caption "reflecting" dup L558).

---

## 3. Detailed point edits

Line numbers refer to the current tex. Edits never change claims, statistics, citation keys, math, or epistemic qualifiers except where the *source of truth* requires it (C- and N-series).

### C — Citations and claim support

**C1 · L92 · high/low — Kosinski figure.**
BEFORE: `\citet{kosinski2024evaluating} reported that GPT-4 solved 95\% of standard false-belief tasks`
AFTER: `\citet{kosinski2024evaluating} reported that GPT-4 solved 75\% of standard false-belief tasks`
WHY: The cited PNAS 121(45):e2405460121 reports 75% (matching 6-year-olds) under its strict all-8-scenarios scoring; 95%/90% appeared only in earlier arXiv preprint versions. The most reviewer-visible error in the paper.

**C2 · L96 + L393 · high/middle — GoEmotions agreement.**
BEFORE (96): `achieving a Krippendorff's $\alpha$ of 0.46 across its fine-grained taxonomy`
AFTER (96): `reporting per-emotion interrater correlations rather than a single aggregate agreement statistic`
BEFORE (393): `GoEmotions \citep{demszky2020goemotions} reports Krippendorff's $\alpha = 0.46$ for 27-category annotation;`
AFTER (393): delete the clause, or replace with a source that actually reports categorical agreement.
WHY: GoEmotions reports no Krippendorff's α anywhere; §4.1 uses Spearman interrater correlations, and 0.46 is Table 4's BERT macro-F1 — a model metric. As written, L393 compares CEI's κ=0.21 against a number that is not an agreement statistic.

**C3 · L96 + L393 · high/middle — SemEval-2007 agreement.**
BEFORE (96): `used just 1K newspaper headlines with 6 emotion categories, achieving modest agreement ($\kappa \approx 0.28$ for fine-grained labels)`
AFTER (96): `used 1,250 newspaper headlines with 6 emotion categories, reporting inter-annotator Pearson correlations of 0.36--0.68 across emotions`
BEFORE (393): `SemEval-2007 Affective Text reports $\kappa \approx 0.28$ for fine-grained emotion classification;`
AFTER (393): delete or restate with the Pearson figures and `\citep{strapparava2007semeval}` (currently uncited at this site).
WHY: S07-1013 §2.3: agreement "carried out using the Pearson correlation measure"; κ never used; 250 dev + 1,000 test headlines.

**C4 · L96 · high/low — EmoBank correlations.**
BEFORE: `reporting Pearson correlations of 0.64--0.72 between the two perspectives`
AFTER: `reporting average inter-annotator Pearson correlations of 0.61 (writer) and 0.63 (reader)`
WHY: Buechel & Hahn 2017 Table 2 reports r as annotator-vs-aggregate agreement per perspective (writer avg 0.605, reader 0.634) — not writer↔reader correlation. Wrong in value and in meaning.

**C5 · L92 + L499 · high/low — Sap et al. numbers and method.**
BEFORE (92): `achieving only 55--65\% accuracy on the SocialIQa benchmark even with chain-of-thought prompting`
AFTER (92): `achieving only 55\% accuracy on SocialIQa and 60\% on ToMi with few-shot prompting`
BEFORE (499): `\citet{sap2022neural} report 55--65\% accuracy on SocialIQa with chain-of-thought prompting`
AFTER (499): `\citet{sap2022neural} report 55\% accuracy on SocialIQa with few-shot GPT-3 prompting`
WHY: The paper's own abstract: "55% and 60% on SocialIQa and ToMi, respectively." No 65% for SocialIQa exists; "chain-of-thought" appears nowhere in the source.

**C6 · L96 · high/low — Bostan & Klinger attribution.**
BEFORE: `identify substantial variation in annotation schemes and agreement levels, noting that agreement tends to be lower with finer-grained taxonomies and that $\kappa$ values between 0.20 and 0.40 are common for emotion tasks`
AFTER: `identify substantial variation in annotation schemes and label granularity, unifying 14 emotion corpora under a common schema`
WHY: The survey contains no inter-annotator-agreement discussion, no κ values, no granularity-vs-agreement claim. The dropped clause is load-bearing for the κ=0.21 defense — if a supporting source is wanted, find one that actually says it.

**C7 · L108 + L437 · middle/low — NRC EmoLex size.**
BEFORE: `validated word-to-Plutchik mappings for $\sim$27K terms`
AFTER: `validated word-to-Plutchik mappings for $\sim$14K English words`
WHY: Mohammad & Turney 2013: 10,170 terms in the described version; expanded version ~24K word–sense pairs / ~14K word types; current release 14,182 unigrams. 27K matches no version under any unit.

**C8 · L393 · high/middle — EmoBench characterization.**
BEFORE: `EmoBench \citep{liu2024emobench} reports $\kappa = 0.852$ but uses a binary emotion recognition format (yes/no) on decontextualized descriptions, a substantially simpler task than...`
AFTER: `EmoBench \citep{sabour2024emobench} reports Fleiss' $\kappa = 0.852$, but on multiple-choice questions designed to have an objectively correct answer, a task that removes precisely the open-ended inference CEI targets`
WHY: κ=0.852 is real (Emotional Application task) but the format is MCQ with curated answer sets, not binary yes/no; scenarios are deliberately contextualized, not "decontextualized." The corrected contrast (constrained-choice vs. open inference) still supports the paragraph's argument honestly.

**C9 · bib `liu2024emobench` · high/low — fabricated authors.**
BEFORE: `author = {Liu, Yuxuan and Zhao, Yang and Li, Ximing and Zhang, Qin}`
AFTER: `author = {Sabour, Sahand and Liu, Siyang and Zhang, Zheyuan and Liu, June M. and Zhou, Jinfeng and Sunaryo, Alvionna S. and Li, Juanzi and Lee, Tatia M. C. and Mihalcea, Rada and Huang, Minlie}`, add `doi = {10.18653/v1/2024.acl-long.326}`, `pages = {5986--6004}`; rename key `sabour2024emobench` (update the one cite at L393).
WHY: None of the four listed names are on the paper (verified via CrossRef + arXiv 2402.12071). Classic hallucinated-citation pattern.

**C10 · bib `hyun2023diplomat` · high/low — fabricated authors.**
BEFORE: `author = {Hyun, Junhyung and Kim, Youngjoong}`
AFTER: `author = {Li, Hengli and Zhu, Song-Chun and Zheng, Zilong}`
WHY: Verified via arXiv 2306.09030; the bib's authors do not exist on this paper.

**C11 · L121, L135 + bib · high/low — PUB uncited.**
ADD bib entry:
```bibtex
@inproceedings{sravanthi2024pub,
  author = {Sravanthi, Settaluri Lakshmi and Doshi, Meet and Kalyan, Tankala Pavan and Murthy, Rudra and Bhattacharyya, Pushpak and Dabre, Raj},
  title = {{PUB}: A Pragmatics Understanding Benchmark for Assessing {LLMs}' Pragmatics Capabilities},
  booktitle = {Findings of the Association for Computational Linguistics: ACL 2024},
  pages = {12075--12097},
  year = {2024},
  doi = {10.18653/v1/2024.findings-acl.719}
}
```
Then cite at the Table 1 PUB column (caption or header) and at L135 (`PUB \citep{sravanthi2024pub} evaluates...`).
WHY: Every number in the PUB column is currently unsourced while its three table-mates are cited.

**C12 · L135 · high/low — DiPlomat mischaracterization.**
BEFORE: `DiPlomat \citep{hyun2023diplomat} targets pragmatic reasoning in diplomatic settings but does not annotate emotions`
AFTER: `DiPlomat \citep{hyun2023diplomat} targets pragmatic reasoning in everyday multi-turn dialogue but does not annotate emotions`
WHY: The name is **Di**alogue + **Prag̲mat**ics; nothing diplomatic about the data. The prose contradicts the paper's own Table 1 ("PIR + CQA").

**C13 · bib `niu2024rethinking` · middle/low.** DOI `10.1109/TAFFC.2024.3519178` returns 404. AFTER: `doi = {10.1109/TAFFC.2025.3584775}`, `year = {2025}` (authors correct). Optional key rename.

**C14 · bib `danescu2013computational` · middle/low.** DOI `10.3115/v1/P13-1025` doesn't exist in CrossRef. AFTER: remove `doi`, add `url = {https://aclanthology.org/P13-1025/}`.

**C15 · L100 · low/low.** BEFORE: `SARC \citep{khodak2018large} provides 1M self-labeled Reddit comments` → AFTER: `...provides 1.3M self-labeled sarcastic Reddit comments`. (iSarcasm "4.5K" is fine: 4,484.)

**C16 · L88 + L499 · middle/low — Hu et al. framing.**
BEFORE (88): `while LLMs match human performance on conventional implicatures, they fall short on non-conventional and context-dependent inferences---precisely the phenomena CEI targets`
AFTER (88): `while large models match human accuracy and error patterns on most pragmatic phenomena, they struggle with those requiring recognition of social expectation violations---precisely the phenomena CEI targets`
(And align the echo at L499.) WHY: The conventional/non-conventional distinction is not Hu et al.'s framing; "social expectation violations" is, and it is actually a *better* fit for CEI.

**C17 · Table 1 (L130) · middle/low.** "Pragmatic subtypes: 14 types" (PUB) counts tasks over 4 phenomena; DiPlomat "2 types" counts its 2 tasks. AFTER: `4 phenomena (14 tasks)` and `2 tasks` (or retitle the row "Tasks/phenomena"). WHY: category confusion a PUB/DiPlomat author-reviewer would flag.

**C18 · L104 · low/low.** Krippendorff does tie thresholds to consequences but also commits to α ≥ .667/.800 cutoffs; the current paraphrase is defensible but is doing load-bearing work for κ=0.21. Consider adding "while maintaining conventional cutoffs for high-stakes coding" or lean on the Plank/Pavlick/Uma line instead.

**C19–C21 · low/low, optional.** Key renames (`mohammad2012semeval`→`mohammad2013crowdsourcing`; `liang2022holistic`→`liang2023holistic`) — cosmetic, prevents exactly the confusion that produced C7. Label Studio → Tkachenko, Malyuk, Holmanyuk, Liubimov (canonical software citation, unverifiable via DOI). OV-MER: optionally sharpen to "236 emotion categories via open-vocabulary annotation." Optional DOI add for `buechel2016corpus`: `10.3233/978-1-61499-672-9-1114`.

### N — Internal numeric consistency

**N1 · L259, L283, L678 · high/low · [AUTHOR] — impossible adjudication count.**
BEFORE (259): `A trained meta-annotator reviewed all flagged scenarios (15.7\% of the dataset, 47 of 300).`
AFTER: `A trained meta-annotator reviewed all flagged scenarios, documenting reasoning for 72 of them (24.0\%).`
Also: Fig. 2 arrow label `15.7\% adjudicated` (L283) → `24.0\% adjudicated`; datasheet `15.7\% of scenarios required expert adjudication` (L678) → `24.0\%`.
WHY: Level 3 flags *all* three-way splits (94) plus VAD-divergent scenarios, so the flagged set is ≥94; 47 is impossible; the only data-grounded count is the 72 documented notes (verified 24+24+24 in three subtype CSVs). Author must confirm what 47 was meant to count.

**N2 · L78, L499, L571, L573, L624 · high/middle · [AUTHOR] — wrong human baseline.**
The recurring comparison `25\% accuracy ... versus 54\% human majority agreement` compares model accuracy against the *proportion of scenarios with a 2-of-3 majority* — not a human accuracy. The comparable human number is **60.9%** (mean annotator-vs-gold; or 68.7% for ≥2-of-3). Either (a) replace 54% with 60.9% at all five sites and restate the gap as ~36 points, or (b) keep 54.3% but rename it unambiguously ("a majority label exists for 54.3% of scenarios") and stop using it as the accuracy comparator. Also resolves N8 (the stray "30-percentage-point gap" at L571 vs "29" elsewhere).

**N3 · L351, L355, L599 · high/low — one term, three metrics.**
"Majority agreement" = exactly-2-of-3 scenarios (54.3%, L351); ≥2-of-3 (deflection "48%", L599, verified 29/60=48.3%); annotator-vs-gold hit rate (deflection 52.2%, L355). Adopt one term per metric — e.g. "majority scenarios (exactly 2 of 3)", "scenarios with any majority", "annotator–gold agreement" — and apply consistently.

**N4 · L355 · high/middle · [AUTHOR] — wrong subtype value + denominators.**
BEFORE: `...mixed signals 60.9\%, and deflection 52.2\%` and `(mean 60.9\%, $N$=15)`
AFTER: mixed signals `58.9\%` (106/180, full denominator); recompute PA consistently (116/180=64.4% full, or 66.7% excluding malformed); mean → `60.4\%` (annotator-level) or drop `$N$=15`.
WHY: 60.9% for mixed signals only arises by silently dropping 2 scenarios with empty golds while PA keeps its malformed rows — two subtypes, two denominators — and it duplicates the quoted mean, suggesting copy-paste. (This line also carries the paper's worst overfull hbox.)

**N5 · data CSVs · high/low · [AUTHOR] — released-data defects (root cause of N4).**
`data/human-gold/data_mixed-signals.csv` ids 171 & 189: empty `gold_standard` (need real adjudication decisions); `data_passive-aggression.csv` id 193: `anticipationa`, id 268: `surpise` (typo fixes). Until repaired, the release has 298 usable golds, contradicting "300 gold labels" (Fig. 2) and the datasheet.

**N6 · L897, L908–912 · high/low — Appendix F rows are not real records.**
BEFORE (897): `Table~\ref{tab:sample-data} shows three representative annotation records from the released CSVs`
The three utterances ("Oh sure, I love working weekends.", "I'm fine, just tired.", "Anyway, who wants coffee?") do not appear anywhere in the released data (verified by substring grep). AFTER: replace rows with the three *verified* records already used in §3.3 (sarcasm id 1: sadness×3→sadness; PA id 18: sadness/surprise/joy→sadness; deflection id 633: surprise/anger/surprise→surprise), or change L897 to "three illustrative records in the release format." WHY: a reviewer who spot-checks the artifact finds this in minutes, and it undercuts the paper's reproducibility posture. Fix first among the N-series.

**N7 · L221 · middle/low.**
BEFORE: `We present five scenarios---one per pragmatic subtype---drawn directly from the released data.`
AFTER: `We present three scenarios drawn directly from the released data, spanning the range of annotator agreement (unanimous, split, majority).`
WHY: only three examples follow, and they are organized by agreement level, not subtype. (Alternative: write the two missing examples — mixed signals, strategic politeness — pulling real records.)

**N9–N12 · low/low.** OOV caption (L441): append "46 OOV responses were observed; the two garbled responses are excluded from the table." Timing: L238 `roughly one hour` → `50--70 minutes`; L923 `median $\approx$ 1 minute` → `mean $\approx$ 1 minute`. L391: add clause "(the largest pair overall, anger$\leftrightarrow$surprise, is non-adjacent)". L573: label the 12.0-pt model spread vs the 12.5-pt random-to-best gap explicitly so they don't read as typos of each other.

### H — Hygiene, mechanics, build

**H1 · L21–24, L51 · high(camera-ready)/low.** `TBD`/`TBD-0000` render in the page-1 header ("Published TBD"), `\editor{TBD}`, OpenReview `?id=TBD`, TODO comment at L22. Fill at submission; grep `TBD|TODO` as a pre-submit gate.

**H2 · repo-level · high/middle · [AUTHOR].** README (L1/4/6) says "NeurIPS 2026 Datasets & Benchmarks"; the paper is DMLR-formatted (`dmlr2e.sty`, `\dmlrheading`, `\editor`, `\impact`, renders "Journal of Data-centric Machine Learning Research"); the repo holds both `cei2026_supplementary_neurips-checklist.tex` and `cei-benchmark-dmlr2026-v1.0.0.zip`. Commit 0ca26a3 flipped the README without the paper following. Decide the venue; reconcile class file, checklist, README, and zip naming.

**H3 · low/low.** Delete `papers/cei2026/figures copy/` — byte-identical duplicate of `figures/` (909 KB), untracked but swept into any zip-based upload.

**H4 · L236, L249 · middle/low.** `Our quality control pipeline (\S\ref{sec:annotation})` self-references §4 from inside §4. Add `\label{sec:qc}` at the §4.2 heading (L249) and retarget.

**H5 · L617 · low/low.** `see footnote~1` is a hardcoded number (currently correct only because `\thanks` renders as ∗). Label the L72 footnote and `\ref` it, or repeat the URL.

**H7–H10 · low.** Build is clean (exit 0, 38 pp, 0 errors/undefined refs). Residual: 5 overfull hboxes (worst 12 pt at tex L355–356, L906, L912 — the sample-data table pushes past the rule); `caption` package doesn't recognize `dmlr2e` (formatting may drift from template — consider dropping `subcaption` if unused); broken `Hfootnote.1` hyperref anchor; unreferenced `app:sample-data`/`app:compute` labels (add signposts from the body; checklists expect a compute pointer); fig5 PNG is 743 KB of the 956 KB PDF (recompress if the venue caps size).

### S — Style / humanization (workflow §4.6 Level A unless noted)

All S-edits are deletion/compression/specificity — no claims, numbers, cites, or qualifiers move. Apply in ONE pass per section; drift budget ≤15–20% of tokens per section; adjudicate each hunk against the §4.7 gate table.

**S1 · L521, L573, L624 · high/middle — triplicated dissociation sentence.**
Keep the full version at L521 (where it is first argued). At L573, compress: `Second, human and model difficulty dissociate across subtypes (Spearman $\rho = -0.50$ between human $\kappa$ and mean model accuracy), as noted above.` At L624 (conclusion), compress to a clause: `...sarcasm is easiest for humans but hardest for models; deflection shows the reverse (\S\ref{sec:eval}).`

**S2 · L428, L562 · high/middle — duplicated VAD-consistency paragraph.**
At L562, drop the restated numbers and lead; keep only the new material:
AFTER (562): `Figure~\ref{fig:emotion-valence} shows the same categorical--dimensional consistency reported in \S\ref{sec:stats}, consistent with \citet{pavlick2019inherent}'s observation that disagreement in subjective tasks is often systematic rather than random.`

**S3 · L96, L110 · high/low — duplicated "CEI differs from…" frame.**
Keep L96's construction; rewrite L110: `What sets CEI apart is the combination of multiple pragmatic subtypes, explicit power relations, and multi-annotator labels with documented agreement, evaluated through contextual emotional inference.`

**S4 · L56 · high/low — abstract opener em-dash wrap.**
BEFORE: `Pragmatic reasoning---inferring intended meaning beyond literal semantics---underpins everyday communication`
AFTER: `Pragmatic reasoning, the ability to infer intended meaning beyond literal semantics, underpins everyday communication`

**S5 · L172, L176, L391 · high/low — em-dash list-wrap mold ×3.**
L172: `...four social settings: workplace (...), family (...), social/friendship (...), and service encounters (...). We chose these settings because indirect speech serves different social functions in each` (also demotes the second dash in that paragraph). L176: parenthesize: `multiple information sources (situational context, social roles, relational history, and communicative norms)`. L391: parenthesize the pairs list.

**S6 · L428, L575, L626 · middle/low.** Keep L575 (has the new regression-metrics detail); delete the L428 sentence; leave L626's brief conclusion callback.

**S7/S8 · L605 vs L630/L632 · middle/middle.** In the `\impact` block, open with a cross-reference to §7.3 and keep only the genuinely new material (educational systems; the power-asymmetry exploitation detail) instead of re-elaborating both three-item lists.

**S9 · L473/L499/L546 · middle/low.** Vary one "not an artifact of X" instance — e.g. L546 → `a robust property of the data rather than a byproduct of who annotated it.`

**S10 · L236/L238 · middle/low.** At L238: `Beyond the categorical and dimensional labels, annotators wrote the free-text explanation described above for each scenario. These explanations are not released...` (definition already given at L236).

**S11 · L238/L322 · middle/low.** At L322: `Per-subtype values reflect the consistent within-subtype rater assignment described in \S\ref{sec:annotation}.`

**S12/S13 · L88/L104 · middle/low.** Drop the vague lead-ins; open with the named citations: `\citet{ruis2023goldilocks} and \citet{shapira2024clever} evaluate whether LLMs exhibit pragmatic competence...`; `Several strands of work challenge the assumption that low agreement is always problematic:` (or open with Passonneau directly).

**S14 · L94 · middle/low.** `We situate CEI within the landscape of emotion-annotated datasets.` → `We compare CEI to related emotion-annotated datasets.`

**S15 · L100 · middle/low.** Drop `Notably,`; replace the closing `, highlighting the gap between surface detection and genuine pragmatic understanding` with ` --- exposing the gap between surface detection and pragmatic understanding` or end the sentence at "intend sarcasm."

**S16/S17 · L90 · middle/middle.** `These benchmarks focus primarily on belief attribution. CEI instead targets emotional inference from pragmatically complex utterances: how the speaker feels, not what they believe.`

**S18 · L573 · middle/low.** Delete the scare-quoted `"diagnostic sweet spot"` label; the two gap numbers that follow already carry the point.

**S19 · L389 · middle/middle.** Fold the bald closer into the preceding sentence: `...often converge on the overall affective direction (overwhelmingly negative, consistent with the pragmatically complex content of CEI scenarios); this pattern is unlikely to arise from random disagreement alone.`

**S20/S21 · L479, L499 · middle/low.** Caption (479): end at `...over zero-shot.` Paragraph (499): cut one of the three stacked -ing interpreters (drop `indicating that even the best models perform unevenly across emotion classes` — the Macro-F1 range says it).

**S22–S32 · low/low (batch pass).** Em-dash pivots → commas at L78, L88 ("...inferences, precisely the phenomena CEI targets"), L96 (split the "misleading---making" sentence). L225: `VAD ratings also diverged across all three dimensions.` L397: split into two sentences. Vary 2–3 "We [verb]" subsection openers (e.g. L408 → `Each annotation also includes dimensional affect ratings.`). Trim the "rather than" family to two uses. L98: `Sarcasm detection is the most heavily studied of these phenomena.` L90: `OpenToM \citep{xu2024opentom} evaluates a wide range of ToM dimensions.` L571: `CEI is a difficult benchmark: current LLMs show large, consistent gaps in pragmatic inference.` L575: `the agreement patterns are systematic rather than random noise.` L599: `This reflects the nature of the task, not a data quality problem.` L558 caption: `Valence skews negative (median $\approx -0.33$); arousal and dominance show broader spread across power relations and subtypes.`

---

## 4. Staged revision plan

Workflow discipline for every stage: one focused commit per stage; word-level diff adjudication per hunk (never section-level accept); the §4.7 drift-gate table; clean rebuild after each stage.

**Stage 1 — Substance: citations and claim support (C1–C17; gate before any style work).**
Bib surgery first (C9, C10, C11, C13, C14), then the prose corrections (C1–C8, C12, C15–C17). The L393 benchmark-comparison paragraph should be rewritten as one unit — three of its four comparators are wrong (C2, C3, C8) — keeping its honest core: *agreement scales inversely with task openness and label granularity, and CEI sits at the open end.* Re-run `/check-citations` afterwards. ~2–3 h.

**Stage 2 — Internal consistency and data (N1–N12) [AUTHOR decisions required].**
Fix the four CSV cells (N5) → recompute L355 (N4) → settle the adjudication count (N1) and the human-baseline question (N2, the one edit that moves the abstract) → unify "majority agreement" terminology (N3) → Table 10 replacement (N6) → N7 → low-tier N9–N12. Regenerate figures/tables from the pipeline (`--stage all_local`) so Fig. 2's "300 gold labels" and the datasheet agree with the data. ~2–4 h + author decisions.

**Stage 3 — Submission hygiene and mechanics (H1–H10).**
Venue decision (H2) drives everything else; then TBDs (H1), cross-ref fixes (H4, H5, H8), delete `figures copy/` (H3), cosmetic build items (H7, H9, H10). ~1–2 h.

**Stage 4 — Humanization (S1–S32), one pass, Level A.**
Order: the five high-criticality structural dedups (S1–S5), then the middle tier, then the S22–S32 batch. Everything here is deletion/compression — no Level B (constrained AI rewrite) appears necessary anywhere in this paper; if a paragraph resists, use the workflow §4.6 prompt with locks, two candidates max. Abstract last, by hand. Budget check: if any section changes >15–20% of tokens, revert and re-scope. ~2–3 h.

**Stage 5 — Validation and freeze.**
Clean `latexmk` rebuild; visual PDF pass (title block, floats, refs, the two overfull-prone tables); `/editor-diff` new-vs-baseline tex as the drift meter of record; one final calibrated `/ai-check` comparing *evidence, not scores*; `grep -n 'TBD\|TODO'` = 0; `/remove-ai-marks` Layer A on shipped artifacts. Stop condition per workflow: every citation resolves and supports its claim, no claim moved, and it reads like the authors.

Suggested commit boundaries: `stage1-citations`, `stage2-consistency` (tex + data + regenerated figures), `stage3-hygiene`, `stage4-style`, each individually revertable.

---

## 5. Overall assessment

**The paper's empirical spine is real and checkable — that is its greatest asset and the reason the citation layer is its greatest liability.** The mechanical audit reproduced essentially every internal statistic from the released CSVs; the build is clean; citations are perfectly balanced with zero orphans. A dataset paper that invites artifact spot-checking, and survives most of them, cannot afford the ones it fails: fabricated author lists on two comparison benchmarks, a related-work section where seven attributed numbers don't survive contact with their sources, an appendix of "released records" that aren't in the release, and an adjudication count that is arithmetically impossible. Any one of these, found by a reviewer, reframes the whole paper's reliability posture — and they are exactly the failure class the guiding workflow ranks above style: *hallucinated citations, drifted claims, chatbot register, in that order.*

**On the humanization axis the paper is in better shape than its detector scores suggest.** All three style auditors converged independently on the same diagnosis: a concrete, numerate, human-sounding empirical core with an AI-assisted interpretive/transitional layer on top. The tells are overwhelmingly *structural* — the same sentence or paragraph appearing two and three times across sections, a repeated em-dash mold, twin positioning closers — which reads less like machine vocabulary and more like a section-by-section drafting process that never got a global dedup pass. This is good news: nearly every style fix is a deletion, the safest edit class, with near-zero claim-drift risk, and the paper will get *shorter* and *stronger* simultaneously.

**Highest-leverage single actions:** (1) fix Kosinski 95→75 and rewrite the L393 comparison paragraph; (2) replace the two fabricated author lists and cite PUB; (3) repair the four CSV cells and the 47/72 adjudication count; (4) replace Appendix F's invented rows with the three verified records; (5) resolve the DMLR-vs-NeurIPS venue split before touching anything venue-formatted. After Stages 1–2 the paper's substance is defensible end-to-end; Stages 3–4 are polish with a favorable risk profile. Estimated total effort: one focused working day plus the author decisions flagged **[AUTHOR]** (human baseline framing, adjudication count, gold labels for ids 171/189, venue).

---

*Sources: six audit-agent reports (bib verification, claim-support verification, three calibrated ai-check style audits, mechanical/consistency audit incl. sandbox build and CSV recomputation), 2026-08-18/20. No edits have been applied to the tex, bib, or data — this report is the workflow's Stage-0 deliverable; all changes above await hunk-level author adjudication.*
