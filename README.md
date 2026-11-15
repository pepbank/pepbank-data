# PepBank open data

PepBank is a free peptide evidence database at <https://pepbank.io>. Each study result in it has the exact quote from its source. This repository holds its open data releases: the peptide records, papers, study results and trials, as files you can download, check and reuse. Community reports, account data and abstracts whose license does not allow reuse are not in the files.

<!-- release:begin counts -->
The release covers 147 peptides, 6 blends and 13 other compounds. It has 97,577 study results from 17,383 studies, and 8,473 clinical trials.
<!-- release:end counts -->

Information, not medical advice.

## Latest release

<!-- release:begin release-table -->
| | |
|---|---|
| Release tag | `2026-11-15` |
| Date | 2026-11-15 |
| Snapshot id | `pepbank-2026-11-15-seq310721` |
| Data version (`data_seq`) | 310721 (updated 2026-11-15) |
| DOI | [10.5281/zenodo.24249757](https://doi.org/10.5281/zenodo.24249757) |

| File | Content | Size | SHA-256 |
|---|---|---|---|
| [`pepbank-2026-11-15-jsonl.tar.gz`](https://github.com/pepbank/pepbank-data/releases/download/2026-11-15/pepbank-2026-11-15-jsonl.tar.gz) | JSONL files, schema, share-alike values, documents | 57.9 MB | ca658e6d74bd8da2543fef1969dbed57eb19b6d8e92a28d73adabb7d57c99612 |
| [`pepbank-2026-11-15.sqlite.gz`](https://github.com/pepbank/pepbank-data/releases/download/2026-11-15/pepbank-2026-11-15.sqlite.gz) | SQLite database with full-text indexes | 130.9 MB | 7e53e1b97a7a84c219e1b50840801bf0fec98d73803657e888eb36938031b9b1 |
| [`manifest.json`](https://github.com/pepbank/pepbank-data/releases/download/2026-11-15/manifest.json) | List of every file with size and SHA-256 | 7.6 KB | cc4615ce28a9633f614239b9876a77a82499a74fdcd62440d893719699778c35 |
| [`SHA256SUMS`](https://github.com/pepbank/pepbank-data/releases/download/2026-11-15/SHA256SUMS) | Checksums for the three files above. The archive holds its own `SHA256SUMS` for the files inside it. | 273 bytes | dcf2d7c7ba1d8c80ba8405b44748953a137347a528fe801def1f06bc7cd6fa2f |
<!-- release:end release-table -->

All releases are on the [releases page](https://github.com/pepbank/pepbank-data/releases). The [changelog](CHANGELOG.md) lists what changed in each one.

## Download and check the files

Download the four files of a release into one folder. In the commands below, `<tag>` and `<snapshot id>` are the values in the table above. On Linux, use `sha256sum` in place of `shasum -a 256`.

```sh
shasum -a 256 -c SHA256SUMS
tar -xzf pepbank-<tag>-jsonl.tar.gz
mkdir -p <snapshot id>/sqlite
gunzip -c pepbank-<tag>.sqlite.gz > <snapshot id>/sqlite/pepbank.sqlite
cd <snapshot id>
shasum -a 256 -c SHA256SUMS
```

The first check covers the release files. The second check covers every file inside the folder. Every line must report `OK`. If one does not, download that file again.

## Read the files

The folder holds `jsonl/`, `sqlite/`, `schema/` and `sharealike/`, and the files `manifest.json`, `SHA256SUMS`, `README.md`, `LICENSE.txt`, `ATTRIBUTION.md` and `CITATION.cff`. Each line of a JSONL file is one record. The [data dictionary](schema/DATA-DICTIONARY.md) explains every field. Run these examples inside the folder.

**JSONL with jq.** Print the name of the first five peptides.

```sh
gunzip -c jsonl/peptides/part-0000.jsonl.gz | head -n 5 | jq -r '.preferred_name'
```

**JSONL with Python.** Count the findings by evidence class.

```python
import glob, gzip, json
from collections import Counter

counts = Counter()
for path in glob.glob("jsonl/findings/part-*.jsonl.gz"):
    with gzip.open(path, "rt", encoding="utf-8") as f:
        for line in f:
            counts[json.loads(line)["evidence_class"]] += 1
print(counts)
```

**SQLite with the sqlite3 shell.** Any SQLite tool reads the file.

```sh
sqlite3 sqlite/pepbank.sqlite "select name, length, evidence_level from peptides order by name limit 5;"
```

## License

The work of PepBank is licensed under [CC BY 4.0](LICENSE). This covers the compilation, the curated peptide records, the normalized fields, and the extracted findings with their statements. You may share and adapt it for any purpose, also commercially. Give credit, link to the license and state your changes.

Two parts are different.

- Abstracts and quotes from articles keep the license of the article. The data includes abstract text only where the article license allows reuse.
- Values from ChEMBL are in the `sharealike/` folder of the archive under [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/). They are not in the CC BY files.

[ATTRIBUTION.md](ATTRIBUTION.md) lists each source and its terms.

## How to cite

<!-- release:begin citation -->
> PepBank. (2026). *PepBank open data, release 2026-11-15 (data version 310721)* [Data set]. Mira Labs LLC. https://doi.org/10.5281/zenodo.24249757
<!-- release:end citation -->

The DOI is on [Zenodo](https://zenodo.org). GitHub shows a "Cite this repository" button that uses [CITATION.cff](CITATION.cff). Also keep the snapshot id (`pepbank-<date>-seq<data_seq>`) in your methods text, so a reader can find the exact data.

## Keep a copy current

A release is a copy of the open records at one data version. To update your copy without a new download, ask the PepBank API for the changes since your data version. `<data_seq>` is the data version in the table above.

```sh
curl "https://pepbank.io/v1/changes?since_seq=<data_seq>"
```

Send the value `next_since_seq` from the answer as `since_seq` in the next call. The API documentation is at <https://pepbank.io/developers>. The OpenAPI file is at <https://pepbank.io/v1/openapi.json>.

## Report an error

Every result links to its source, and AI can make mistakes. If you find a wrong value, [open a data error issue](https://github.com/pepbank/pepbank-data/issues/new?template=data-error.yml). Give the record id or page URL, the field, what is wrong and the source text that shows it. [CONTRIBUTING.md](CONTRIBUTING.md) has the details. Open data errors that the latest release still has are in [docs/KNOWN-ISSUES.md](docs/KNOWN-ISSUES.md).

## Links

- Site: <https://pepbank.io>
- Open data page: <https://pepbank.io/open-data>
- How PepBank works: <https://pepbank.io/methods>
- Methods in this repository: [docs/METHODS.md](docs/METHODS.md)
- Quality tables: [docs/QUALITY.md](docs/QUALITY.md)
- API: <https://pepbank.io/developers>
- MCP server: <https://pepbank.io/mcp>

PepBank is run by Mira Labs LLC.
