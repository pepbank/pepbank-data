# Known issues

These are the open data errors that the latest release still has. Each row says what is wrong and what happens next. "Planned fix: release 2" means the fix is not in this release. "Under review" means the issue is not decided yet.

<!-- release:begin known-issues-release -->
Applies to release 2026-11-15 (data version 310721).
<!-- release:end known-issues-release -->

To report a new error, use the [data error form](https://github.com/pepbank/pepbank-data/issues/new?template=data-error.yml).

| No. | Where | What is wrong | Status |
|---|---|---|---|
| 1 | Sample sizes written as words | In some papers the abstract gives the number of subjects in words, for example "Thirty-two male Sprague-Dawley rats" (paper `PUB-42542926`, findings `FND-4254292610` to `FND-4254292617`) or "seventy two Wistar Albino male rats" (paper `PUB-18594781`). The sample size of these findings is empty and marked "not reported in source". A text search finds this pattern in about 974 papers with 4,522 findings. A hand check of 25 random cases found a real subject count in 23 of them. These numbers are an estimate from a text pattern, not a full count. | Planned fix: release 2 |
| 2 | Trial TRIUMPH-2 (`NCT05929079`), paper `PUB-42810372`, 21 findings | The sample sizes of these findings are 571 to 581. They are the size of one comparison (for example 292 plus 287 gives 579). The enrollment of the trial is 1,152. A reader can take the finding value for the trial size. This is a definition: `sample_size` is the size of the comparison, not the trial enrollment. The [data dictionary](../schema/DATA-DICTIONARY.md) says so. | Under review |
| 3 | Semaglutide data | The outcome group "Blood pressure" is under the body system "Endocrine and hormones". In the retatrutide data the same outcome group is under "Cardiovascular". | Under review |
| 4 | Thymosin alpha-1 data | The outcome group "Immune response" for animal findings is under the body system "Nervous system and cognition". The human and laboratory cell findings of the same group are under "Immune and inflammation". | Under review |
| 5 | Change feed of the API (`GET /v1/changes`) | The field `changed_fields` is empty on most events. Newer events can list the changed fields. The event itself and its record id are correct. | Under review |
