# How the PepBank data is made

PepBank collects studies and trials from public sources. An AI model writes down what each study found, with the sentence it comes from. Software and a second AI model check every result. This page describes each step. The measured quality is in [QUALITY.md](QUALITY.md).

## Sources

| Data | Source |
|---|---|
| Studies | Europe PMC |
| Publication types | PubMed |
| Trials | ClinicalTrials.gov |
| Retractions | Retraction Watch, read through Crossref |
| Peptide details | UniProt, PubChem, GSRS and ChEMBL |
| Approval and sport status | FDA, EMA and WADA |

Each source keeps its own license. See [ATTRIBUTION.md](../ATTRIBUTION.md). Study searches look at titles and abstracts only. The abstract is the short summary at the top of a paper.

PepBank also offers a live search of Europe PMC. Live results are labelled "Live" and are not stored as study results.

## Linking studies to peptides

A program matches each study and each trial to a peptide by name and alias. The match method and the confidence are kept with the match. A match can be wrong, for example when two molecules share a short name.

## How papers are selected

PepBank does not read every study at once. A fixed rule picks the studies to read first. A study needs an abstract of at least 200 characters, an active status and a clear link to the peptide. Studies rank by study design first (meta-analysis, systematic review, randomized trial and so on), then by citations, then by date.

Studies are selected in rounds. A study counts once, in the first round that selected it. Some rounds took up to 60 papers for each of 30 featured peptides, with at most 6 narrative reviews for each. Other rounds took every remaining paper of a peptide. Others took papers that PubMed tags as human studies, or human-use papers that a title-and-abstract screen picked. Papers that the scope rules leave out are not selected.

## How results are read

An AI model reads the title and the abstract of one study. It writes down each result. A result that shows no effect is also a result. For each result the model must copy an exact quote from the text. If the abstract does not state a value, the value stays empty. The model does not follow instructions that it finds inside a paper. It writes no result for background, aims, hypotheses, or findings that the study cites from other studies.

Each result gets a peptide role, an evidence class and a study design. The written instructions have version numbers. QUALITY.md lists the versions used.

## How results are checked

Software runs these checks on every result:

1. The result has every required field, and each value is an allowed value.
2. Each quote is an exact copy of text in the title or the abstract.
3. Every number in a statement appears in the title or the abstract. A measurement value that is not found is removed before release.
4. The result names a peptide that the study is linked to.
5. A result whose role is not "administered" has no dose, route or duration. A "natural levels" statement says it is the body's own peptide.

Then a second AI model, separate from the first, reads the abstract and the result. It accepts, fixes or rejects each result. A rejected result is not released. A stronger AI model also re-checks samples, and every study of the type "Review" that kept results. QUALITY.md names the AI models used.

A model can still make a mistake that both checks miss.

## Evidence classes

Every result has one class. The class names the kind of study. It is not a quality grade.

| Class | Meaning |
|---|---|
| Human | Studies in people. |
| Animal | Studies in live animals. |
| Laboratory | Cells, tissue or test tubes. |
| Predicted | Predictions. Nothing was measured. The site shows this class as "Computer model". |

A result in animals or cells does not show that the same effect occurs in people. PepBank does not pool results or give an overall verdict on a peptide.

## Peptide roles

| Role | Meaning | Counts as evidence for taking the peptide |
|---|---|---|
| Administered | The study gave the peptide to people or animals, or put it on cells, tissue or molecules. | Yes |
| Natural levels | The study measured the body's own peptide. Nobody was given it. | No |
| Other | The peptide was a tool, a carrier for another drug, part of a combination, or the subject of a method such as an assay. | No |

Results with the role Natural levels or Other have their own sections. They never count toward key results, evidence counts or charts. A result from before roles existed counts as administered.

## Scope rules

PepBank leaves out some topics on purpose. A program reads the title, the abstract and the MeSH terms of each study with fixed words and fixed steps. The same study always gets the same answer. The rule never deletes a study. Results from a left-out study do not appear in pages, counts, exports or snapshots. Left-out studies are not counted in the measured quality.

One rule exists now. Oxytocin studies about childbirth (labour, delivery, the time after birth, caesarean section, uterine contraction, miscarriage or abortion care, pregnancy, lactation or breastfeeding) are left out for oxytocin. Oxytocin studies on other topics stay in scope.

## Retractions and corrections

PepBank takes publication types and notices from Europe PMC and compares every study with the Retraction Watch list. Each notice is kept as a dated status event with its source.

A retracted or withdrawn study stays in the data with a warning. Its results never count as evidence. A study with an expression of concern or a correction shows a notice.

## The counting rule

A result counts only when it has passed the checks and the status of its source is `active` or `corrected`. A published correction does not make a study invalid. Results from studies that are retracted, withdrawn, removed from the source or under an expression of concern do not count as key results, in evidence counts or in charts.

Key results follow a fixed order: studies in people first, then stronger study designs, then larger, more cited and newer studies. One study cannot fill every place.

## What the data does not hold

* Community reports, user data and account data.
* Results from the full text of a paper. Only the title and the abstract are read.
* Results for studies that were not selected or not yet read. An empty list of results is not evidence of no effect.
* Abstracts whose article license does not allow reuse.

A dose in a result is the dose that one study used, in that species and by that route. It is not a dose recommendation. This is not medical advice.

To report an error, see [CONTRIBUTING.md](../CONTRIBUTING.md).
