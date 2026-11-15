# Data dictionary

This file describes every field of the records in the JSONL and SQLite files. It follows `schema/peptide-db.schema.json`. The text in the Meaning column comes from the schema. Where the schema has no description, this file says so.

Read these points first.

- The Example column shows the format of a value. The examples are not copies of real records.
- A dotted name such as `claim.outcome` means the field `outcome` inside the object `claim`. A name with `[]` such as `aliases[].name` means a field inside each item of a list.
- A field that a source may not report is an object with a `status` and a `value`. See "Value that a source may not report" below.
- `sample_size` is the size of the comparison that a finding describes. It is not the enrollment of the trial. Trial enrollment is in the `enrollment` field of the trial record. For example, a trial with an enrollment of 1,152 can have findings with a sample size of 579, the two arms (292 and 287) that one comparison uses.
- The record type `peptide_profile` is not in the schema. The file `peptide_profiles` holds the curated profile of each peptide (summaries, targets, regulatory status). This file does not describe it.
- The field `extraction.self_reported_confidence` is internal. The data files leave it out.
- Fields with the name `meta` hold version data. See "Meta" below.

## Record types

| Record type | File in the JSONL archive | Schema definition |
|---|---|---|
| Peptide | `jsonl/peptides/` | `peptide` |
| Publication | `jsonl/publications/` | `publication` |
| Passage | `jsonl/passages/` | `passage` |
| Finding | `jsonl/findings/` | `finding` |
| Trial | `jsonl/trials/` | `trial` |

## Peptide

One distinct substance. A fragment or a modified analog gets its own record and a relation to its parent. Aliases never merge two substances.

| Field | Type | Meaning | Example |
|---|---|---|---|
| `record_type` | string (constant) | not described in the schema | `"peptide"` |
| `peptide_id` | string | not described in the schema | `PEP-000123` |
| `status` | string | not described in the schema. Allowed values: active, merged, withdrawn. | `active` |
| `merged_into` | string or null | not described in the schema | `null` |
| `preferred_name` | string | not described in the schema | `BPC-157` |
| `kind` | string | not described in the schema. Allowed values: natural_peptide, natural_fragment, synthetic_analog, synthetic_fragment, modified_form, metal_complex, conjugate, mixture, undefined_product, small_molecule. | `synthetic_fragment` |
| `structure` | object | not described in the schema || (object, see rows below) |
| `structure.structure_status` | string | not described in the schema. Allowed values: defined, partially_defined, disputed, undisclosed. | `defined` |
| `structure.sequence` | string or null | One-letter canonical residues. X marks a non-canonical residue that the modifications list describes. | `GEPPPGKPADDAGLV` |
| `structure.length_residues` | integer or null | not described in the schema | `15` |
| `structure.helm` | string or null | HELM notation when a source gives it. HELM can describe non-canonical monomers and links. | `null` |
| `structure.modifications` | array of object | not described in the schema || (list, see rows below) |
| `structure.modifications[].mod_type` | string | not described in the schema. Allowed values: n_terminal_acetylation, c_terminal_amidation, non_canonical_residue, d_amino_acid, residue_substitution, lipidation, pegylation, cyclization, disulfide_bond, albumin_binding_moiety, metal_complex, other. | `c_terminal_amidation` |
| `structure.modifications[].position` | integer or null | not described in the schema | `15` |
| `structure.modifications[].position_label` | string | Readable position, for example 'N-term', 'Lys26', 'position 8'. | `N-term` |
| `structure.modifications[].residue` | string or null | not described in the schema | `K` |
| `structure.modifications[].detail` | string | not described in the schema | (free text) |
| `structure.modifications[].provenance` | array of provenance entry | Where the value came from. See Provenance entry below. || (list, see rows below) |
| `structure.structure_note` | string | not described in the schema | (free text) |
| `structure.provenance` | array of provenance entry | Where the value came from. See Provenance entry below. || (list, see rows below) |
| `aliases` | array of object | not described in the schema || (list, see rows below) |
| `aliases[].name` | string | not described in the schema | `Body Protection Compound-157` |
| `aliases[].normalized` | string | Lookup key. Unicode NFKD with combining marks removed, lower case, Greek letters spelled in Latin letters (alpha, beta, gamma ...), then only Unicode letters and digits are kept: no spaces, no hyphens, no symbols. Letters of other scripts stay ("Семакс" gives "семакс"). A name that gives an empty key is not stored. Used for lookup only. | `body protection compound 157` |
| `aliases[].alias_type` | string | not described in the schema. Allowed values: inn, usan, code_name, trade_name, vendor_name, common_name, abbreviation, systematic_name, cas_like_number. | `common_name` |
| `aliases[].ambiguous` | boolean | True when the same name points to more than one peptide record. The alias_resolution record lists the candidates. | `false` |
| `aliases[].provenance` | array of provenance entry | Where the value came from. See Provenance entry below. || (list, see rows below) |
| `relations` | array of object | not described in the schema || (list, see rows below) |
| `relations[].relation` | string | not described in the schema. Allowed values: fragment_of, analog_of, modified_form_of, complex_of, salt_form_of, precursor_of, same_substance_candidate. | `fragment_of` |
| `relations[].target` | object | not described in the schema || (object, see rows below) |
| `relations[].target.peptide_id` | string or null | not described in the schema | `PEP-000045` |
| `relations[].target.uniprot_accession` | string or null | not described in the schema | `P12345` |
| `relations[].target.positions` | null or object | Range of the parent in the numbering of uniprot_accession (UniProt numbering, 1-based, both ends included). The position of the analog inside the parent peptide record is stored in the peptide profile as parent.parent_positions. || (object, see rows below) |
| `relations[].target.positions.start` | integer | not described in the schema | `1` |
| `relations[].target.positions.end` | integer | not described in the schema | `15` |
| `relations[].target.label` | string | not described in the schema | (free text) |
| `relations[].basis` | string | not described in the schema | (free text) |
| `relations[].provenance` | array of provenance entry | Where the value came from. See Provenance entry below. || (list, see rows below) |
| `xrefs` | object | not described in the schema || (object, see rows below) |
| `xrefs.uniprot` | array of string | not described in the schema | `["P12345"]` |
| `xrefs.chembl` | array of string | not described in the schema | `["CHEMBL12345"]` |
| `xrefs.pubchem_cid` | array of string | not described in the schema | `["123456"]` |
| `xrefs.unii` | array of string | not described in the schema | `["ABCDE12345"]` |
| `xrefs.dbaasp` | array of string | not described in the schema | `["DBAASP_123"]` |
| `xrefs.drugbank` | array of string | not described in the schema | `["DB12345"]` |
| `xrefs.cas` | array of string | CAS Registry Numbers. | `["137525-51-0"]` |
| `xrefs.pdb` | array of string | Protein Data Bank entry IDs (4 characters) that contain this peptide or its parent. | `["1ABC"]` |
| `xrefs.kegg` | array of string | KEGG DRUG or KEGG COMPOUND IDs. | `["D12345"]` |
| `xrefs.iuphar` | array of string | IUPHAR/BPS Guide to PHARMACOLOGY ligand IDs. | `["1234"]` |
| `eligibility` | object | not described in the schema || (object, see rows below) |
| `eligibility.rule_version` | string | not described in the schema | (rule id) |
| `eligibility.eligible` | boolean | not described in the schema | `true` |
| `eligibility.criteria` | array of object | not described in the schema || (list, see rows below) |
| `eligibility.criteria[].criterion` | string | not described in the schema | (rule name) |
| `eligibility.criteria[].result` | string | not described in the schema. Allowed values: pass, fail, unknown. | `pass` |
| `eligibility.criteria[].detail` | string | not described in the schema | (free text) |
| `popularity` | null or object | not described in the schema || (object, see rows below) |
| `popularity.rule_version` | string | not described in the schema | (rule id) |
| `popularity.score` | number | not described in the schema | `12.5` |
| `popularity.rank` | integer or null | not described in the schema | `3` |
| `popularity.components` | object | not described in the schema | (object of numbers) |
| `popularity.computed_at` | string | not described in the schema | `2026-10-30T13:33:00Z` |
| `meta` | object | Version data. A change never overwrites a version. valid_from_seq and valid_to_seq place the version on the change feed, so a query can run 'as of' a sequence number. See Meta below. | (see Shared structures) |

## Publication

One stored paper. The record holds metadata, status and rights. It does not hold findings.

| Field | Type | Meaning | Example |
|---|---|---|---|
| `record_type` | string (constant) | not described in the schema | `"publication"` |
| `publication_id` | string | not described in the schema | `PUB-12345678` |
| `ids` | object | not described in the schema || (object, see rows below) |
| `ids.pmid` | string or null | not described in the schema | `12345678` |
| `ids.pmcid` | string or null | not described in the schema | `PMC1234567` |
| `ids.doi` | string or null | not described in the schema | `10.1000/example` |
| `ids.openalex` | string or null | not described in the schema | `W1234567890` |
| `ids.nct` | array of string | not described in the schema | `["NCT01234567"]` |
| `title` | string | not described in the schema | (title as published) |
| `journal` | string or null | not described in the schema | (journal name) |
| `publication_types` | array of string | not described in the schema | `["Randomized Controlled Trial"]` |
| `peptides` | array of peptide link | Tracked peptides that the paper mentions. It links stored papers without findings (layer 2) to a peptide. Each entry is a Peptide link with scope fields. Fields listed below. || (list, see rows below) |
| `dates` | object | not described in the schema || (object, see rows below) |
| `dates.published` | string or null | not described in the schema | `2024-03` |
| `dates.pubmed_entrez` | string or null | not described in the schema | `2024-03-05T00:00:00Z` |
| `dates.first_seen_by_service` | string | not described in the schema | `2026-10-01T08:00:00Z` |
| `status` | string | not described in the schema. Allowed values: active, corrected, expression_of_concern, retracted, withdrawn, removed_from_source. | `active` |
| `status_events` | array of object | not described in the schema || (list, see rows below) |
| `status_events[].event` | string | not described in the schema. Allowed values: correction, retraction, expression_of_concern, withdrawal, removal_from_source, reinstatement. | `correction` |
| `status_events[].notice_ids` | object | not described in the schema || (object, see rows below) |
| `status_events[].notice_ids.pmid` | string or null | not described in the schema | `12345678` |
| `status_events[].notice_ids.doi` | string or null | not described in the schema | `10.1000/notice` |
| `status_events[].effective_date` | string or null | not described in the schema | `2024-05-01` |
| `status_events[].detected_at` | string | not described in the schema | `2026-10-02T04:00:00Z` |
| `status_events[].source` | string | not described in the schema. Allowed values: pubmed, pmc_oa, pubtator3, europepmc, crossref, retraction_watch, openalex, clinicaltrials_gov, chembl, uniprot, pubchem, gsrs, dbaasp, apd, dramp, thpdb, iuphar_gtopdb, manual_curation, internal_extraction, internal_policy. | `retraction_watch` |
| `status_events[].source_field` | string | not described in the schema | (source field name) |
| `text_rights` | object | Rights per text layer. The service stores and returns each layer only as its license allows. || (object, see rows below) |
| `text_rights.metadata` | string | Rights class of one stored value. The access layer reads it before it returns the value. | `cc0` |
| `text_rights.abstract` | object | not described in the schema || (object, see rows below) |
| `text_rights.abstract.license_class` | string | Rights class of one stored value. The access layer reads it before it returns the value. | `cc_by` |
| `text_rights.abstract.stored` | boolean | not described in the schema | `true` |
| `text_rights.abstract.returned_to_clients` | boolean | not described in the schema | `true` |
| `text_rights.full_text` | object | not described in the schema || (object, see rows below) |
| `text_rights.full_text.available` | boolean | not described in the schema | `false` |
| `text_rights.full_text.license_class` | string | Rights class of one stored value. The access layer reads it before it returns the value. | `unknown` |
| `text_rights.full_text.license_url` | string or null | not described in the schema | `null` |
| `text_rights.full_text.stored` | boolean | not described in the schema | `false` |
| `text_rights.full_text.returned_to_clients` | boolean | not described in the schema | `false` |
| `processing` | object | Data layer of the paper. extracted = layer 1 (checked findings). queued and stored_only = layer 2 (stored without AI processing). || (object, see rows below) |
| `processing.extraction_status` | string | not described in the schema. Allowed values: extracted, queued, stored_only. | `extracted` |
| `processing.reason` | string or null | not described in the schema | `null` |
| `processing.queued_at` | string or null | not described in the schema | `null` |
| `processing.extracted_with` | string or null | Extraction prompt version, or null. | (prompt version) |
| `source_records` | array of provenance entry | Where each stored value came from. One entry per source. See Provenance entry below. | (see Shared structures) |
| `meta` | object | Version data. A change never overwrites a version. valid_from_seq and valid_to_seq place the version on the change feed, so a query can run 'as of' a sequence number. See Meta below. | (see Shared structures) |

## Passage

An exact span of a source text. The span points into one text version (`text_version_hash`). If the source text changes, the hash no longer matches and the finding is queued for review.

| Field | Type | Meaning | Example |
|---|---|---|---|
| `record_type` | string (constant) | not described in the schema | `"passage"` |
| `passage_id` | string | not described in the schema | `PSG-0000123456` |
| `publication_id` | string | not described in the schema | `PUB-12345678` |
| `section` | string | not described in the schema. Allowed values: title, abstract, methods, results, discussion, table, figure_legend, registry_record, database_record, other. | `abstract` |
| `text_version_hash` | string | not described in the schema | (64 hex characters) |
| `char_start` | integer | not described in the schema | `120` |
| `char_end` | integer | not described in the schema | `310` |
| `quote_sha256` | string | not described in the schema | (64 hex characters) |
| `text` | string or null | Null when the rights class does not allow the service to return the text. Offsets and hashes stay, so a client with source access can check the span. | (exact quote) |
| `text_withheld_reason` | string or null | not described in the schema. Allowed values: rights, plan, null. | `rights` |
| `meta` | object | Version data. A change never overwrites a version. valid_from_seq and valid_to_seq place the version on the change feed, so a query can run 'as of' a sequence number. See Meta below. | (see Shared structures) |

## Finding

One claim from one source. Findings are never merged. Disagreeing findings link to each other in `conflicts`.

| Field | Type | Meaning | Example |
|---|---|---|---|
| `record_type` | string (constant) | not described in the schema | `"finding"` |
| `finding_id` | string | not described in the schema | `FND-1234567801` |
| `publication_id` | string | not described in the schema | `PUB-12345678` |
| `peptide` | object (peptide link) | Link from a mention in a source to a peptide record. peptide_id is null when the mention does not resolve. Each entry is a Peptide link. See Peptide link below. | `{"peptide_id":"PEP-000123", ...}` |
| `evidence_class` | string | not described in the schema. Allowed values: human, animal, laboratory, predicted. | `animal` |
| `evidence_detail` | object | not described in the schema || (object, see rows below) |
| `evidence_detail.design` | string | not described in the schema. Allowed values: randomized_controlled_trial, non_randomized_trial, observational_study, case_report_or_series, systematic_review, animal_in_vivo, ex_vivo_tissue, in_vitro_cell, in_vitro_cell_free, in_silico, other. | `animal_in_vivo` |
| `evidence_detail.species` | object (status, value) | A value that a source may not report. A missing value keeps its reason. It is never an empty string. | `{"status":"reported","value":"rat"}` |
| `evidence_detail.model_system` | object (status, value) | A value that a source may not report. A missing value keeps its reason. It is never an empty string. | `{"status":"reported","value":"Achilles tendon transection"}` |
| `evidence_detail.sample_size` | object (status, value) | The size of the comparison that the finding describes (for example, the arms that the result compares). It is not the enrollment of the whole trial. Trial enrollment is in the trial record, in enrollment. The schema has no further description. | `{"status":"reported","value":32}` |
| `claim` | object | not described in the schema || (object, see rows below) |
| `claim.claim_type` | string | not described in the schema. Allowed values: efficacy_outcome, safety_outcome, pharmacokinetics, activity_measurement, mechanism, other. | `efficacy_outcome` |
| `claim.outcome` | string | not described in the schema | (free text) |
| `claim.direction` | string | not described in the schema. Allowed values: increase, decrease, no_significant_change, mixed, not_applicable, not_reported. | `increase` |
| `claim.comparator` | object (status, value) | A value that a source may not report. A missing value keeps its reason. It is never an empty string. | `{"status":"reported","value":"saline"}` |
| `claim.statement` | string | Short normalized statement written by the extractor. It is not a quote. | (short statement) |
| `claim.statement_plain` | string | Plain-language restatement of the statement for non-specialists (v0.3). It adds no facts and no numbers that the statement does not hold. | (plain-language statement) |
| `claim.outcome_group` | string | Normalized outcome group, for example Wound healing or Body weight (v0.3). Evidence maps count findings by this value. | `Wound healing` |
| `claim.body_system` | string | Body system of the outcome, for example Musculoskeletal or Metabolic and weight (v0.3). Whole body when no system fits. | `Musculoskeletal` |
| `claim.analysis_role` | string | Role of the analysis in the source. not_assessed = the extraction prompt did not ask (pilot, prompt v0.1). Allowed values: primary, secondary, exploratory, not_stated, not_assessed. | `primary` |
| `claim.comparator_basis` | string | What the direction compares. not_assessed = the extraction prompt did not ask (pilot, prompt v0.1). Allowed values: between_groups, change_from_baseline, single_arm_no_comparator, not_applicable, not_stated, not_assessed. | `between_groups` |
| `claim.peptide_role` | string | Whether the study gave the peptide (administered), measured the body's own peptide (endogenous_level) or did not test the peptide itself (other): the peptide was a tool, a carrier or targeting ligand for another drug, part of a combination, a challenge test, or the subject of a method such as an assay or a synthesis. Extraction prompt v0.4. Only administered findings are evidence about taking the peptide. The service never presents an endogenous_level finding as an effect of the peptide. Allowed values: administered, endogenous_level, other. | `administered` |
| `measurements` | array of object | not described in the schema || (list, see rows below) |
| `measurements[].quantity` | string | For example MIC, IC50, half_life, body_weight_change_percent. | `body_weight_change_percent` |
| `measurements[].value` | object (status, value) | A value that a source may not report. Status says why a value is missing. | `{"status":"reported","value":-8.2}` |
| `measurements[].value_high` | number or null | Upper bound when the source gives a range. | `null` |
| `measurements[].qualifier` | string | not described in the schema. Allowed values: =, <, <=, >, >=, ~, range. | `=` |
| `measurements[].unit` | string or null | Unit as written in the source. Null when the source states no unit (pilot finding, 2026-10-04). | `%` |
| `measurements[].unit_ucum` | string or null | not described in the schema | `%` |
| `measurements[].condition` | string | not described in the schema | (free text) |
| `intervention` | object | not described in the schema || (object, see rows below) |
| `intervention.dose` | object (status, value) | A value that a source may not report. A missing value keeps its reason. It is never an empty string. | `{"status":"reported","value":"10 ug/kg"}` |
| `intervention.route` | object (status, value) | A value that a source may not report. A missing value keeps its reason. It is never an empty string. | `{"status":"reported","value":"subcutaneous"}` |
| `intervention.duration` | object (status, value) | A value that a source may not report. A missing value keeps its reason. It is never an empty string. | `{"status":"reported","value":"14 days"}` |
| `source_passages` | array of object | not described in the schema || (list, see rows below) |
| `source_passages[].passage_id` | string | not described in the schema | `PSG-0000123456` |
| `source_passages[].role` | string | not described in the schema. Allowed values: primary, supporting, design_context. | `primary` |
| `extraction` | object | not described in the schema || (object, see rows below) |
| `extraction.method` | string | not described in the schema. Allowed values: llm, rules, manual, source_structured. | `llm` |
| `extraction.model` | string or null | not described in the schema | (model name) |
| `extraction.prompt_version` | string or null | not described in the schema | (prompt version) |
| `extraction.extractor_version` | string | not described in the schema | (version) |
| `extraction.run_id` | string | not described in the schema | (run id) |
| `extraction.extracted_at` | string | not described in the schema | `2026-10-01T08:00:00Z` |
| `extraction.self_reported_confidence` | number or null | Internal. Never returned to clients. Never used as a gate or a ranking input. Left out of the data files. | (not in the data files) |
| `validation` | object | not described in the schema || (object, see rows below) |
| `validation.status` | string | out_of_scope: the paper is out of scope for this peptide by a scope policy (for example oxytocin_childbirth_v1). The finding is stored with its history, but PepBank does not check it, and readers do not show it. It is not a review error. Allowed values: unvalidated, auto_checked, human_verified, rejected, disputed, out_of_scope. | `auto_checked` |
| `validation.scope_previous_status` | string | Only with status out_of_scope: the status before the scope policy applied. When the link is in scope again, the finding returns to this status. Allowed values: unvalidated, auto_checked, human_verified, rejected, disputed. | `auto_checked` |
| `validation.checks` | array of object | not described in the schema || (list, see rows below) |
| `validation.checks[].name` | string | not described in the schema | (check name) |
| `validation.checks[].passed` | boolean | not described in the schema | `true` |
| `validation.checks[].detail` | string | not described in the schema | (free text) |
| `validation.reviewed_by` | string or null | not described in the schema | `null` |
| `validation.reviewed_at` | string or null | not described in the schema | `null` |
| `conflicts` | array of object | not described in the schema || (list, see rows below) |
| `conflicts[].finding_id` | string | not described in the schema | `FND-1234567802` |
| `conflicts[].conflict_type` | string | not described in the schema. Allowed values: opposite_direction, value_disagreement, significance_disagreement, identity_disagreement. | `opposite_direction` |
| `conflicts[].note` | string | not described in the schema | (free text) |
| `source_status` | string | Copied from the publication at each version. Retracted findings stay in the database and are excluded from default searches. Allowed values: active, corrected, expression_of_concern, retracted, withdrawn, removed_from_source. | `active` |
| `meta` | object | Version data. A change never overwrites a version. valid_from_seq and valid_to_seq place the version on the change feed, so a query can run 'as of' a sequence number. See Meta below. | (see Shared structures) |

## Trial

One ClinicalTrials.gov registry record. A trial is not a finding and not a publication.

| Field | Type | Meaning | Example |
|---|---|---|---|
| `record_type` | string (constant) | not described in the schema | `"trial"` |
| `trial_id` | string | TRL- plus the ClinicalTrials.gov NCT number. | `TRL-NCT01234567` |
| `ids` | object | not described in the schema || (object, see rows below) |
| `ids.nct` | string | not described in the schema | `NCT01234567` |
| `ids.secondary_ids` | array of string | Sponsor study ID first, then registry secondary IDs. | `["ABC-123"]` |
| `title` | string | not described in the schema | (title as published) |
| `peptides` | array of peptide link | Peptides that the trial names. Each entry is a Peptide link. See Peptide link below. || (list, see rows below) |
| `overall_status` | string | not described in the schema. Allowed values: active_not_recruiting, completed, enrolling_by_invitation, not_yet_recruiting, recruiting, suspended, terminated, withdrawn, available, no_longer_available, temporarily_not_available, approved_for_marketing, withheld, unknown. | `completed` |
| `phases` | array of string | not described in the schema | `["phase2"]` |
| `study_type` | string | not described in the schema. Allowed values: interventional, observational, expanded_access. | `interventional` |
| `enrollment` | object (status, value, type) | A registry number that can be actual or estimated, for example enrollment. | `{"status":"reported","value":1152,"type":"actual"}` |
| `conditions` | array of string | not described in the schema | `["Obesity"]` |
| `interventions` | array of object | not described in the schema || (list, see rows below) |
| `interventions[].type` | string | not described in the schema. Allowed values: drug, device, biological, procedure, radiation, behavioral, genetic, dietary_supplement, combination_product, diagnostic_test, other. | `drug` |
| `interventions[].name` | string | not described in the schema | (intervention name) |
| `sponsor` | object | not described in the schema || (object, see rows below) |
| `sponsor.name` | string | not described in the schema | (sponsor name) |
| `sponsor.class` | string | not described in the schema. Allowed values: nih, fed, other_gov, indiv, industry, network, ambig, other, unknown. | `industry` |
| `dates` | object | not described in the schema || (object, see rows below) |
| `dates.first_posted` | object (status, value, type) | A registry date. type is null when the registry gives no actual/estimated flag. | `{"status":"reported","value":"2023-06-01","type":"actual"}` |
| `dates.start` | object (status, value, type) | A registry date. type is null when the registry gives no actual/estimated flag. | `{...}` |
| `dates.primary_completion` | object (status, value, type) | A registry date. type is null when the registry gives no actual/estimated flag. | `{...}` |
| `dates.completion` | object (status, value, type) | A registry date. type is null when the registry gives no actual/estimated flag. | `{...}` |
| `dates.last_update_posted` | object (status, value, type) | A registry date. type is null when the registry gives no actual/estimated flag. | `{...}` |
| `results_posted` | boolean | not described in the schema | `true` |
| `linked_publication_ids` | array of string | Publications in the database that the registry record cites. | `["PUB-12345678"]` |
| `source_records` | array of provenance entry | Where each stored value came from. One entry per source. See Provenance entry below. | (see Shared structures) |
| `meta` | object | Version data. A change never overwrites a version. valid_from_seq and valid_to_seq place the version on the change feed, so a query can run 'as of' a sequence number. See Meta below. | (see Shared structures) |

## Shared structures

### Value that a source may not report

Used for fields such as `evidence_detail.species`, `evidence_detail.sample_size`, `claim.comparator`, `intervention.dose` and `measurements[].value`.

| Field | Type | Meaning | Example |
|---|---|---|---|
| `status` | string | Why the value is there or missing. Allowed values: reported, not_reported_in_source, not_extracted, not_applicable, withheld_rights. | `reported` |
| `value` | string, number or null | The value. It is null when the status is not `reported`. It is never an empty string. | `"rat"` |

### Registry value (trial `enrollment` and `dates.*`)

| Field | Type | Meaning | Example |
|---|---|---|---|
| `status` | string | Allowed values: reported, not_reported_in_source. | `reported` |
| `value` | integer or date string | A registry number or a registry date. Null when the status is `not_reported_in_source`. | `1152` |
| `type` | string or null | Allowed values: actual, estimated, null. The schema says that `type` is null when the registry gives no actual or estimated flag. | `actual` |

### Peptide link

Used in `finding.peptide`, `trial.peptides[]` and `publication.peptides[]`.

| Field | Type | Meaning | Example |
|---|---|---|---|
| `peptide_id` | string or null | not described in the schema. The schema says that `peptide_id` is null when the mention does not resolve to a peptide record. | `PEP-000123` |
| `mention_text` | string | not described in the schema | `BPC 157` |
| `resolution_method` | string | not described in the schema. Allowed values: exact_alias, normalized_alias, sequence_match, source_structured, manual, llm. | `exact_alias` |
| `resolution_confidence` | string | not described in the schema. Allowed values: high, medium, low, unresolved. | `high` |
| `scope` | string | Only in `publication.peptides[]`. Scope policy of this link. Allowed values: in_scope, out_of_scope. | `in_scope` |
| `scope_rule` | string or null | Only in `publication.peptides[]`. Id of the scope rule that put the link out of scope. Null when the link is in scope. | `null` |

### Provenance entry

Used in every `provenance` list and in `source_records`.

| Field | Type | Meaning | Example |
|---|---|---|---|
| `source` | string | not described in the schema. Allowed values: pubmed, pmc_oa, pubtator3, europepmc, crossref, retraction_watch, openalex, clinicaltrials_gov, chembl, uniprot, pubchem, gsrs, dbaasp, apd, dramp, thpdb, iuphar_gtopdb, manual_curation, internal_extraction, internal_policy. | `europepmc` |
| `source_record_id` | string or null | not described in the schema | `12345678` |
| `source_version` | string or null | Release number, update file name or index date of the source. | `2026-10-30` |
| `retrieved_at` | string | not described in the schema. A date-time. | `2026-10-30T13:33:00Z` |
| `method` | string | not described in the schema. Allowed values: api, bulk_file, manual, derived. | `api` |
| `license_class` | string | Rights class of one stored value. The access layer reads it before it returns the value. Allowed values: public_domain, cc0, cc_by, cc_by_sa, cc_by_nc, cc_by_nc_nd, publisher_copyright, terms_restricted, unknown. | `cc0` |

### Meta

Used in the field `meta` of every record. Version data. A change never overwrites a version. `valid_from_seq` and `valid_to_seq` place the version on the change feed, so a query can run "as of" a sequence number.

| Field | Type | Meaning | Example |
|---|---|---|---|
| `meta.version` | integer | not described in the schema | `1` |
| `meta.valid_from_seq` | integer | not described in the schema. See the text above. | `308560` |
| `meta.valid_to_seq` | integer or null | not described in the schema. See the text above. | `null` |
| `meta.created_at` | string | not described in the schema. A date-time. | `2026-10-30T13:33:00Z` |
| `meta.record_hash` | string | not described in the schema. 64 hexadecimal characters. | `(64 hex characters)` |
