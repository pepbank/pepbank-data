# Contributing

## Report a data error

The most useful contribution is a report of a wrong value. Use the [data error form](https://github.com/pepbank/pepbank-data/issues/new?template=data-error.yml). Please give:

- The record id (for example `FND-1234567801`) or the URL of the page on pepbank.io.
- The name of the field that is wrong.
- What is wrong.
- The source text that shows it. A quote or a link to the study helps most.

Search the open issues first, and read [docs/KNOWN-ISSUES.md](docs/KNOWN-ISSUES.md). A duplicate report is not a problem, but a comment on the existing issue is faster.

When a report is right, the value is corrected or removed in PepBank. The change appears in the next release and in the change feed (`GET https://pepbank.io/v1/changes`).

## Pull requests

The data files are not in git. They are release assets. Pull requests are for fixes to the documents in this repository, such as a typo in the README or a clearer sentence in the data dictionary. A pull request cannot change the data.

## Questions

Use the form at <https://pepbank.io/contact>. For the API, read <https://pepbank.io/developers>.

## Security and privacy

Do not report a security or privacy problem in a public issue. See [SECURITY.md](SECURITY.md).

## Conduct

All participation follows the [Code of Conduct](CODE_OF_CONDUCT.md).
