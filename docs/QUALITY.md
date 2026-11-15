# Measured quality

This page shows how often the second AI model corrected the first. The numbers come from the PepBank quality report generated on 28 October 2026 (02:08 UTC). The report is built from 1,738 check files. See [METHODS.md](METHODS.md) for the steps.

| Item | Value |
|---|---|
| Selection rule | select_v0.1 |
| Instruction versions used | extract-v0.4, v0.5, v0.6, v0.7 |
| Current instruction version | extract-v0.7 |
| Studies selected | 20,868 |
| Peptides covered | 136 |
| Batch size | 12 studies |
| Check files | 1,738 (3 pilot, 1,735 full run) |

The report names these AI models for reading: Claude Sonnet 5.5, GPT-5.6 Terra and GPT-6.1 Sol. It names the same three for the second step.

## Coverage

| Item | Value |
|---|---|
| Studies selected | 20,868 |
| Studies read, checked and imported | 19,480 |
| Share of selected studies | 93.3% |
| Results written | 100,634 |

Studies with at least one checked result: 17,557.

## Last import

| Item | Value |
|---|---|
| Studies | 19,480 |
| Results released | 97,624 |
| Fixed by the second AI model | 28,303 |
| Rejected by the second AI model | 2,898 |
| Failed the code checks | 0 |

## Check totals

A second AI model read each result against its abstract and gave a verdict.

| Measure | Value |
|---|---|
| Results checked | 99,610 |
| Accepted as written | 68,509 |
| Fixed | 28,068 |
| Rejected | 3,033 |
| Critical corrections (a reader would have been misled) | 8,734 |
| Critical corrections caused by a later rule | 629 |
| Results the first model missed, added by the second | 1,024 |
| Rejected on re-check by a stronger AI model | 1,035 |

## Rates

| Rate | Value |
|---|---|
| Accepted as written | 68.8% |
| Usable (accepted or fixed) | 97.0% |
| Critical error rate | 8.8% |
| Critical error rate, later-rule changes left out | 8.1% |
| Recall (results found by the first model) | 99.0% |

The critical rate counts mistakes that the second AI model or the re-check caught in the first model's work. The later-rule number counts results that followed the rules of their time and changed when a newer rule was applied. These rates are not a guarantee for other peptides or studies.

## By group of check files

| Group | Check files | Studies | Results | Accepted | Fixed | Rejected | Critical | Added |
|---|---|---|---|---|---|---|---|---|
| Pilot | 3 | 35 | 195 | 174 | 20 | 1 | 4 | 0 |
| Full run | 1,735 | 18,064 | 99,415 | 68,335 | 28,048 | 3,032 | 8,730 | 1,024 |
| All | 1,738 | 18,099 | 99,610 | 68,509 | 28,068 | 3,033 | 8,734 | 1,024 |

## What the second AI model corrected

One correction can have several error types.

| Error type | Count |
|---|---|
| Measurement | 9,158 |
| Sample size | 5,604 |
| Plain-language statement | 5,600 |
| Statement | 5,250 |
| Other | 5,189 |
| Study design | 3,528 |
| Comparator | 3,332 |
| Peptide role | 2,929 |
| Direction of the effect | 2,615 |
| Quote | 2,061 |
| Outcome group | 1,756 |
| Analysis role | 1,267 |
| Population | 1,224 |
| Not the study's own result | 837 |
| Body system | 553 |
| Wrong peptide | 511 |
| Evidence class | 306 |

## Pilot

Two AI models (Haiku and Sonnet) read the same 36 studies on 9 October 2026, with instruction version extract-v0.4. The same second AI model checked both.

| Measure | Haiku | Sonnet | Target |
|---|---|---|---|
| Results written | 145 | 195 | |
| Accepted as written | 91 (62.8%) | 174 (89.2%) | |
| Fixed | 51 | 20 | |
| Rejected | 3 | 1 | |
| Usable | 97.9% | 99.5% | at least 95% |
| Critical errors | 6 (4.1%) | 4 (2.1%) | at most 5% |
| Results missed | 12 | 0 | |
| Recall | 92.2% | 100% | at least 90% |
| Meets all targets | Yes | Yes | |

The pilot is small, so its rates are rough.
