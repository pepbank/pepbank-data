# Attribution

Cite the release by its snapshot id (see `manifest.json` and [CITATION.cff](CITATION.cff)). Credit the sources below when you reuse their data.

The data comes from public sources. Each stored value names its source and the license class of that value, in the `source_records` and `provenance` fields. The archive of each release also holds its own `ATTRIBUTION.md` with the number of source entries for that release.

## Sources

| Source | What PepBank uses | License |
|---|---|---|
| Europe PMC | Studies, abstracts, retraction notices | Metadata CC0. Abstracts keep the article license. |
| PubMed | Publication types and MeSH terms | NLM data. Articles keep their copyright. |
| ClinicalTrials.gov | Registered trials and their posted results | ClinicalTrials.gov terms |
| Retraction Watch | Retractions, through Crossref | Crossref terms |
| UniProt | Parent protein links | CC BY 4.0 |
| PubChem | Compound identifiers and formulas | Free to use. Some entries have their own terms. |
| GSRS (FDA and NCATS) | Substance identity and sequences | CC0 |
| ChEMBL | Some identifiers and development phases | CC BY-SA 3.0 |
| FDA, EMA and WADA | Approval and sport status, with the date | Public regulator pages |

## Notes by source

- **Europe PMC and PubMed**: bibliographic metadata (identifiers, titles, journals, dates, publication types, MeSH terms, citation counts). Abstract text is included only where the article license allows reuse (CC BY or CC0). The license of each paper is in its record, under `text_rights`.
- **ClinicalTrials.gov** (U.S. National Library of Medicine): trial records. Attribute ClinicalTrials.gov as the source, show the date of processing (the `retrieved_at` field of each source entry) and state your changes. The records here were reformatted into the PepBank schema.
- **Retraction Watch**: retraction signals in the `status_events` of publications. Credit the Retraction Watch database when you reuse them.
- **GSRS (FDA/NCATS Global Substance Registration System)**: substance identity and sequence data in peptide records.
- **UniProt** (CC BY 4.0): parent protein and peptide range links.
- **PubChem** (National Center for Biotechnology Information): compound identifiers and formulas. Depositors of individual records can have their own terms.
- **ChEMBL** (EMBL-EBI, CC BY-SA 3.0): not in the CC BY files. Values from ChEMBL are in the `sharealike/` folder of the archive. If you adapt them, share your adaptation under CC BY-SA 3.0 and give credit to ChEMBL. Do not merge these values into data that you license under other terms.

## Third-party text

Abstract and quote text keeps the license of its source article. Abstracts are in the data only where the article license allows reuse.

## Information, not medical advice

The data describes published research. It does not recommend a dose or a treatment.
