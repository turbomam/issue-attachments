# Ingest run — `nmdc_nmdc_linkml_store` (2026-04-25)

## What I did

1. **Wrote the notebook** at `~/nmdc-lakehouse/lakehouse/nmdc_linkml_store_ingest.ipynb` (15 cells, generated via `nbformat`).

   Configuration:
   - `DATA_DIR = Path("~/nmdc-lakehouse/lakehouse").expanduser()`
   - `TENANT = "nmdc"`, `DATASET = "nmdc_linkml_store"`, `BUCKET = "cdm-lake"`
   - `MODE = "overwrite"`, `CHUNK_TARGET_GB = 20`
   - Resolved namespace: `nmdc_nmdc_linkml_store`

2. **Replaced the off-cluster bootstrap with on-cluster clients**, per the request.
   Instead of `ingest_lib.initialize()` (which checks SSH tunnels, starts pproxy, spawns a
   JupyterHub server) the Setup cell calls the real `berdl_notebook_utils`:

   ```python
   from berdl_notebook_utils import get_spark_session, get_minio_client
   spark        = get_spark_session()
   minio_client = get_minio_client()
   ```

   `~/BERIL-research-observatory/scripts` is then prepended to `sys.path` so the ingest
   helpers (`detect_source_files`, `build_table_stats`, `upload_files`, `run_ingest`,
   `verify_ingest`) import from `ingest_lib`.

3. **Repaired the `berdl_notebook_utils` submodule attrs after importing `ingest_lib`.**
   `ingest_lib`'s import-time setup unconditionally sets

   ```python
   sys.modules["berdl_notebook_utils.setup_spark_session"].get_spark_session = None
   sys.modules["berdl_notebook_utils.clients"].get_minio_client = None
   ```

   to make off-cluster usage safe. On JupyterHub those submodules are real, so I rebound
   them to the live clients after import:

   ```python
   sys.modules["berdl_notebook_utils.setup_spark_session"].get_spark_session = lambda **kw: spark
   sys.modules["berdl_notebook_utils.clients"].get_minio_client = lambda **kw: minio_client
   ```

   `data_lakehouse_ingest.core` reads these as fallbacks when no `spark` / `minio_client`
   is passed; `run_ingest` always passes them, so this is belt-and-suspenders.

4. **Executed the notebook in place**:

   ```
   jupyter nbconvert --to notebook --execute --inplace \
       --ExecutePreprocessor.timeout=3600 nmdc_linkml_store_ingest.ipynb
   ```

   Total wall time ≈ 2 min 25 s.

## Result

All 13 Parquet files uploaded to MinIO bronze and written to Delta silver in the
`nmdc_nmdc_linkml_store` namespace. Delta row counts match the source Parquet row counts
exactly:

| table                       | rows         |
|-----------------------------|--------------|
| biosample_set               |       16,640 |
| calibration_set             |          197 |
| configuration_set           |           20 |
| data_generation_set         |       12,026 |
| data_object_set             |      265,005 |
| field_research_site_set     |          110 |
| functional_annotation_agg   |   54,348,408 |
| instrument_set              |           18 |
| manifest_set                |           69 |
| material_processing_set     |       19,572 |
| processed_sample_set        |       20,776 |
| study_set                   |           84 |
| workflow_execution_set      |       30,440 |
| **Total**                   | **54,734,365** |

Verified by reading row counts from each `.parquet` via `pyarrow.parquet.read_metadata`
and comparing them against `SELECT COUNT(*)` from each Delta table.

## Caveat — `verify_ingest` is not Parquet-aware

The `verify_ingest()` step in the notebook printed `MISMATCH` for every table. This is
**not** a real problem. `ingest_lib.build_table_stats` calls `_count_lines` (which counts
`\n` bytes) on every source file. That works for TSV/CSV but is meaningless for binary
Parquet — the byte count of `\n` characters in a Parquet file has no relation to its row
count. The Delta side is correct; the "expected" baseline is the artifact.

A clean fix would patch `ingest_lib.build_table_stats` to special-case `*.parquet` and
read `pyarrow.parquet.read_metadata(f).num_rows` instead of calling `_count_lines`. Not
done here — flagged for follow-up.

## Output paths

- Bronze (Parquet): `s3a://cdm-lake/tenant-general-warehouse/nmdc/datasets/nmdc_linkml_store/`
- Silver (Delta):   `s3a://cdm-lake/tenant-sql-warehouse/nmdc/nmdc_nmdc_linkml_store.db`
- Progress log:     `s3a://cdm-lake/tenant-general-warehouse/nmdc/datasets/nmdc_linkml_store/_ingest_progress.jsonl`
- Ingest config:    `s3a://cdm-lake/tenant-general-warehouse/nmdc/datasets/nmdc_linkml_store/config/nmdc_linkml_store.json`
