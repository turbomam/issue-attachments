# MicroFlora Danica changesheets

Attachments for
[microbiomedata/nmdc-ingest-agent issue 58, "Create changesheet with credit associations for MFD study"](https://github.com/microbiomedata/nmdc-ingest-agent/issues/58).
Study `nmdc:sty-11-2c3s5473`.

## Use these two, and load both or neither

| file | what it does |
|---|---|
| [`mfd-principal-investigator-2026-08-20.tsv`](mfd-principal-investigator-2026-08-20.tsv) | sets `principal_investigator` to Mads Albertsen with `orcid:0000-0002-6151-190X` |
| [`mfd-credit-associations-2026-08-24.tsv`](mfd-credit-associations-2026-08-24.tsv) | inserts 71 `has_credit_associations` |

**Submit the principal investigator sheet first.** The two are not independent. The study page
matches the principal investigator to a credit association by ORCID
(`web/src/components/TeamInfo.vue` in `microbiomedata/nmdc-server`), and when the slot is empty that
comparison is against an empty string, so it matches the first contributor who also has no ORCID.
Ten of the 71 have none. Loading credit associations alone would therefore put a Principal
Investigator badge on someone who is not one, on a public page, without failing anything. Loading
both makes Albertsen's ORCID match his own credit association and the label lands correctly.

Applied to dev on 2026-08-27 and verified. Not applied to production as of that date.

## Do not use these

| file | why |
|---|---|
| `mfd-credit-associations-2026-08-20.tsv` | Superseded 2026-08-24. Differs from the current sheet by one line: it spells author `v8` "Yuhong Yang" where the paper, PubMed and [ORCID 0000-0001-5634-1427](https://orcid.org/0000-0001-5634-1427) all give "Yu Yang". **Both files validate clean**, so nothing downstream will catch the wrong choice. |
| `mfd-credit-associations-2026-08-07.tsv` | Superseded 2026-08-20. 72 associations rather than 71: it includes a credit association for the consortium itself, which cannot currently be expressed, because a credit role can only be given to a person. See [nmdc-schema issue 3375](https://github.com/microbiomedata/nmdc-schema/issues/3375). |
| `mfd-principal-investigator-2026-08-07.tsv` | Superseded 2026-08-20. |

These files are kept rather than deleted because
[issue 58](https://github.com/microbiomedata/nmdc-ingest-agent/issues/58) links the two 2026-08-07
files with `blob/main` URLs, which break on rename or delete. Four other links in that thread are
pinned to a commit SHA and would survive. **Do not rename or move anything in this directory without
checking the links in issue 58 first.**

Two earlier files, `mfd-credit-associations-2026-07-23.tsv` and
`mfd-principal-investigator-2026-07-23.tsv`, are named in the first comments on issue 58 but have
never existed in this repository.

## Why this file exists

Five near-identical sheets sit here, issue 58 names seven filenames across six comments spanning
four dates, and the issue body names none. The current pair is not guessable from the dates: the
newest credit sheet is 2026-08-24 while the newest principal investigator sheet is 2026-08-20.
Picking the wrong one is the failure mode that validation cannot catch.
