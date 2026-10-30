# PitWall

![status](https://img.shields.io/badge/status-building-orange)

Race data pipeline: NASCAR JSON feeds to S3 and Snowflake with Airflow and dbt

**Stack:** Python, Apache Airflow, AWS S3, Snowflake, dbt, GitHub Actions

> **Status: Building.** Launching soon.

## Highlights

- Airflow DAG that fetches race lists, results, and lap times from NASCAR's public JSON feeds for the Cup, O'Reilly, and Truck series
- Raw JSON landed in S3, partitioned by season and race, then loaded into Snowflake VARIANT tables with COPY INTO
- Historical backfill plus daily incremental loads
- Idempotent loads: MERGE on natural keys, so reruns add zero duplicate rows
- dbt star schema (dim_driver, dim_track, dim_race, fact_result, fact_lap) built from staging, intermediate, and mart layers
- dbt tests (unique, not_null, relationships, accepted_values, plus custom row-count and lap-time range tests)
- GitHub Actions runs `dbt build` against a CI schema on every pull request
- dbt source freshness checks and a schema-drift check that fails the DAG when a feed adds or removes fields
- Typed Python with pytest coverage for the extract code

Benchmarks will be published here once completed.

## Layout

```
dags/       Airflow DAGs (fetch, land in S3, load to Snowflake)
src/        Typed Python extract and load code
tests/      pytest tests for the extract code
dbt/        dbt project: staging, intermediate, marts, tests
bench/      benchmark scripts and results
docs/       design notes (idempotency, MERGE vs append, VARIANT flattening)
.github/    CI workflows
```

## Roadmap

- [ ] Phase 1: confirm NASCAR feeds and local setup (Airflow on Docker Compose, Snowflake, S3)
- [ ] Week 1: Airflow DAG, S3 landing, Snowflake raw tables, backfill, idempotent MERGE
- [ ] Week 2: dbt star schema, tests, GitHub Actions CI
- [ ] Week 3: freshness and schema-drift checks, docs, benchmarks
