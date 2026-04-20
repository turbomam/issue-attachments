# Pre-NUC-migration repo sweep

**Date:** 2026-04-20
**Source machine:** LBL MacBook Pro (MAM-M74)
**Target machine:** Ubuntu NUC (192.168.0.204)
**Scope:** all 85 git repos in `~/Documents/gitrepos/`, checking for uncommitted working-tree changes, unpushed commits on the current branch, and local branches without an upstream.

---

## Unpushed commits (real risk — would be lost if not pushed)

| Repo | Branch | Commits ahead |
|---|---|---|
| `llmenv` | `explore-isd` | **32** |
| `llmenv` | `main` | **27** |
| `nmdc-schema` | `eecavanna-fix-docker-app-container-on-linux-java-version` | 9 |
| `nmdc-schema` | `1419-when-are-shared-elements-allowed-what-about-in-owl` | 7 |
| `biosample-enricher` | `96-oceanography` | 2 |
| `artl-mcp` | `77-switch-build-process-from-twine-back-to-hatch` | 1 |
| `envo` | `1487-make-release-so-new-stream-terms-will-be-visible-on-ols-etc` | 1 |
| `oak-mcp` | `32-align-with-artl-mcps-buildtestpublish-apporaches` | 1 |

---

## Local-only branches (no upstream — invisible to other machines)

- `artificial-intelligence-ontology`: `removed-src-ontology-artifacts`
- `attributes-of-biosamples`: `gh-pages`
- `biosample-enricher`: `50-ultimate-integration-test-retrieve-biosample-metadata-from-gold-and-nmdc-normalize-put-though-lookups-and-quantify-successcoverage`, `autoformat-hool`
- `bsdbng`: `issue-70-branch-safety-rules`
- `envo`: `issue-1415-4`
- `env-embeddings`: `46-complete-ncbi-dataset`, `random_forest_predict_envo`
- `external-metadata-awareness`: `cavanna-json-tabulate`, `feature/gold-studies-seq-projects-flattening`
- `identifiers-sandbox`: `gh-pages`
- `nmdc-schema`: `before-reversion`, `fluid-salvage`, `metamodel-reconciliation`, `pr-2653`, `pr-2689`, `pr-2690`, `ucum-temp`
- `submission-schema`: `gh-pages`, `issue-12-ttl`, `mam_reorientation`

---

## Dirty working trees — likely meaningful

### Large / in-progress work
- **`llmenv`** (branch `explore-isd`) — 67+ pending changes. Major refactor in progress.
- **`CMM-AI`** — 49 pending changes, mostly `D` (deletions) of publication `.md` / `.pdf` files under `data/publications/`.
- **`PFAS-AI`** — 28+ pending changes, similar publication deletions.
- **`nmdc-ontology`** — ontology import rebuilds and subset TSV deletions under `src/ontology/`.
- **`crawl-first`** (branch `gold-nmdc-enrichment`) — 26+ changes including large archive files (`cache.tar`, `data.tar`, `logs.tar`) that should probably *not* be copied to NUC.
- **`mcp_literature_eval`** — 24+ new eval-result YAMLs under `results/` + new project configs.
- **`nmdc-schema`** (branch `add-linkml-linting`) — generated pydantic/schema/JSON artifacts modified, plus new discussion/report drafts (`adr-ucum-implementation-updated.md`, `github-discussion-post-{1,2}.md`, `linkml_lint_output.txt`, `ontology_prefix_usage*`).

### Generated-artifact drift (may be discardable)
- **`attributes-of-biosamples`** — regenerated `project/*` artifacts + schema/datamodel.
- **`biolink-subset`** — regenerated `project/*` artifacts.
- **`identifiers-sandbox`** (branch `ids-and-pythonless`) — regenerated artifacts + new `SpecificThing-valid.yaml`, `instantiate_and_jsonschema.py`.

### Smaller but possibly real
- **`aurelian`** — new `src/gafp.py` (added + modified).
- **`bacphen-awareness`** — new `halophily_investigation_report.md`.
- **`bioproject-py-mongo`** — Makefile/README + `list_mongodb_paths.py` modifications.
- **`bsdbng`** — new `release-v0.2.0/` and `release-v0.3.0/` dirs.
- **`context-collaboration`** — new `asserted_and_inferred_mappings.tsv`, `inferences_from_cumulative.tsv`, `long_from_wide.{json,tsv}`.
- **`eDNAAqua-Plan`** — new `poetry.lock`, `pyproject.toml`.
- **`eq-expr-sandbox`** — datamodel regenerated + new `_version.py`.
- **`fitness-mcp`** — new `SESSION_SUMMARY.md`.
- **`linkml-model`** — pending deletion of `linkml_model/model/schema/no_orphans.yaml`.
- **`linkml-runtime`** — new `mcp_schema_server.py`, `python_interpreters_report.md`.
- **`metacoder`** — new `.claude.json`, `.claude.json.backup` (possible secrets — check before committing).
- **`mixs-legacy`** — new `tree.tsv`, `tree.txt`.
- **`nmdc-mixs-supplement-for-envo`** — Makefile + classes-by-grid-expansion.py + `nmsfe_static.ttl` + new `ubergraph_org_substance_SC_query.sparql`.
- **`nmdc-runtime`** — new `Gb0188393.json`, `Makefile.gold`, `fetch_gold_biosample.py`, `session_summary.md`.
- **`nmdc-server`** — new `BIOSAMPLE_SEARCH_API_RESEARCH.md`.
- **`oak-mcp`** — new `_version.py`.
- **`ontogpt`** — new `debug_cache.py`, `test_cache.py`, `chem_utilization_*` templates, `poetry.lock`, `.gene_requests_cache.sqlite`.
- **`sample-annotator`** — new `existing_study_ids.csv`, `jgi_biosample_ids.csv`, `jgi_goldstudyids.csv`, `missing_study_ids.txt`, `session-summary.md`.
- **`schema-automator`** — poetry.lock/pyproject.toml + new `d3o*` schema files, `.DS_Store`, pending deletion of `bacdive_strains.schema.json`.
- **`submission-schema`** (branch `tools/env-triad-audit`) — new `enum_permissible_values.tsv`, `extract_enum_pvs.py`, `temp_target/`.
- **`to-duckdb`** — new `session-summary.md`.
- **`use-nmdc-schema`** — new `instantiate_biosample.py`, `instantiate_database_with_metagenome_sequencing_activity_set.py`, `json_of_class.py`, `test.py` + modifications.

---

## Likely noise (safe to ignore)

- `.DS_Store` in `issue-attachments`, `issues`, `metacoder`, `schema-automator`, `bsdbng`
- `.idea/` in `cicd-container`, `eq-expr-sandbox`, `to-duckdb`, `w3id.org`, `semantic-sql`
- `__pycache__/` in `eDNAAqua-Plan`
- Auto-regenerated `_version.py` files
- `poetry.lock` / `uv.lock` drift on repos whose `pyproject.toml` wasn't also changed

---

## Recommended pre-migration actions

1. **Push unpushed commits** — especially `llmenv` (59 commits across two branches).
2. **Review local-only branches** — decide which are WIP worth pushing vs. abandoned.
3. **Stash or commit work-in-progress** in the big dirty trees (`llmenv`, `nmdc-schema`, `crawl-first`, `mcp_literature_eval`) so the NUC clone can pick it up.
4. **Explicitly decide about large untracked artifacts** that are not in git and are not being committed (tarballs under `crawl-first/nmdc-with-ornl/archives/`, eval caches, etc.) — copy separately via Samba share if needed, or leave behind.
5. **`metacoder/.claude.json*`** — check for secrets before committing or copying.
