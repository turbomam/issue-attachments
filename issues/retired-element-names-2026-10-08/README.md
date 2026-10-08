# Retired nmdc-schema element names in microbiomedata repos, 2026-10-08

Supports an issue in https://github.com/microbiomedata/issues asking repo owners to remove stale mentions.

- `retired-names.txt`: 377 names that some nmdc-schema release defined and the current schema does not. One per line, for `rg -w -F -f`.
- `retired-names.tsv`: the same names with element kind, the last release that had each one, and whether `deprecated.yaml` records it.
- `per-repo.tsv`: the 40 non-archived repos with at least one mention, with the date of the last commit, the number of matching lines, the most frequent names, commits in the 12 months before 2026-10-08, the person with the most of those commits, and the person with the most content commits. A content commit changes at least one file outside `.github/` and lockfiles, so GitHub Actions upgrades and dependency bumps don't count. `documented_maintainer` gives the maintainer named in the repo's own docs, where there is one.
- `mentions.tsv`: one row per name per matching line, with a permalink to the scanned commit.

## How the list was made

The names come from `assets/schema_element_history/retired_elements.tsv` in https://github.com/microbiomedata/nmdc-schema/pull/3478, which compares every nmdc-schema release tag with `main` at 647010882. The list keeps names with an underscore, CamelCase, or a leading capital letter. It leaves out:

- plain lowercase words such as `soil`, which match ordinary text
- permissible values
- 81 names that MIxS `main` still defines, such as `lib_screen`, because MIxS terms are out of scope

## How the repos were scanned

On 2026-10-08, each non-archived repo's default branch was cloned at depth 1 and searched with:

```sh
rg --no-ignore --hidden -g '!.git' -n -w -F -f retired-names.txt .
```

Matches are whole words and case-sensitive, and binary files were skipped. A match is a lead, not a confirmed problem. Some names are ordinary capitalized words, such as `Solution` and `Activity`.
