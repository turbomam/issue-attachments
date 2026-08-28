# issue-attachments

Sidecar repo for verbose GitHub issue/PR content that doesn't belong in the issue body.

## What goes here

- Log output, stack traces, linter dumps
- Options-analysis tables (more than 3 alternatives)
- Schema comparison matrices
- Screenshots supporting a before/after claim
- Data files an issue needs to link to, such as changesheets
- Any content that would push an issue body past ~1500 characters

## How to use

1. Put the file under a directory named for the repo whose issue it belongs to.
2. Commit it on a branch and open a PR rather than committing to `main`.
3. Link it from the issue, pinning the commit SHA rather than `main`:
   `https://github.com/turbomam/issue-attachments/blob/{sha}/{path}`

On Mark's machines a global hook (`core.hooksPath = ~/bin/git-hooks`) enforces step 2 by refusing
direct commits to a default branch. That hook is not part of this repo, so it does not apply to
anyone else. Follow step 2 anyway.

SHA-pinned links keep resolving if the file is later moved, renamed, or removed, and they do not
change meaning when the file is revised. A `blob/main` link silently starts pointing at different
content, or breaks.

## Naming

Files are named for their content and carry a date, so a name matches how the thing is referred to
in discussion and a later revision does not overwrite an earlier one. Where the date belongs varies:
on the file when a single file stands alone, on the directory when a set of files shares one
occasion. Existing names are not uniform in delimiter or casing, and that is tolerated rather than
enforced. Examples in this repo:

```
nmdc-ingest-agent/mfd-credit-associations-2026-08-20.tsv
mixs-ols-2026-06-24/tree_before.png
nmdc-lakehouse/INGEST_RUN_2026-04-25.md
```

Superseded versions stay. `nmdc-ingest-agent/` holds both the 2026-08-07 and 2026-08-20 changesheets
so the earlier shape remains visible from the comment that linked it.

## A directory README when the files are not self-explaining

A directory holding several near-identical files needs a `README.md` saying which to use.
`nmdc-ingest-agent/README.md` is the worked example: five changesheets sat there with nothing
distinguishing them, the issue named seven filenames across six comments and four dates, and the
current pair was not guessable from the dates because the newest credit sheet was 2026-08-24 while
the newest principal investigator sheet was 2026-08-20. Two of them differed by a single line and
both validated clean, so nothing downstream would catch the wrong choice.

What that README does, and what a directory README should do:

- name the current file or files, and say what each one does
- say plainly if they must be used together, and what goes wrong if they are not
- list the superseded ones with the reason each was replaced, rather than deleting them
- link the issue the whole directory serves

Mark it superseded in the README rather than renaming or deleting the file. Renaming breaks any
`blob/main` link pointing at it, and issue 58 has two of those alongside four SHA-pinned ones.

A directory README describes current state, so link it at `main`. The attachments themselves are
pinned to a SHA. That difference is deliberate: a pinned README would freeze at the moment it was
written and go stale as files are added.

## Do not delete or move files casually

Every file here may be linked from an issue body somewhere. Removing or renaming one breaks that
link permanently, and the link is the reason the file exists. Before touching a file, find the issue
that motivated it; the commit message usually names it.

## Why this repo exists

NMDC team retro (2026-04-14): stop dumping large amounts of LLM-generated text into issues. This
repo keeps issue bodies concise while preserving the detailed material for anyone who wants it.

## Not everything here fits

Some root-level files predate the per-repo convention and are not issue attachments at all
(`S25_migration_reconnections.md`, `lexar_microsd_specs.md`, the 2026-04-20 pre-NUC-migration
inventories). They are left in place rather than moved, per the rule above.

If you need to know whether one of them is still linked from anywhere, search before assuming it is
not:

```
gh search code --owner turbomam 'issue-attachments/blob' | grep <filename>
```

An unlinked stray can be moved into a dated directory. A linked one stays where it is, whatever the
convention says, because the link is the reason the file exists.
