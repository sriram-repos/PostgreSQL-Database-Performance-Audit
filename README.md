# PostgreSQL-Database-Performance-Audit
Creating a new PostgreSQL database to load CIC_IDS2018 dataset containing approx. 17 million rows with 80 features. The repository will include scripts for ingestion,setting baselines,performance tuning,database audit checks and all related scripts.

Data Source - [Kaggle ](https://www.kaggle.com/datasets/dhoogla/csecicids2018)
File Type - parquet files

Kaggle Parquet Files --> Python Ingestion script using Kaggle API --> PostgreSQL Staging Database --> Trim usuable columns to Fact Tables --> Create a Star Schema

| Strategy / Setup | Query Type | Est Storage Size | Est Latency (sec) | Est Speedup |
| :--- | :--- | :--- | :--- | :--- |
| **Stage 0:** Heap Scan (No Indexes) | 24-hr Threat Vol. Aggregation | 7.8 GB | `38.420 s` | Baseline |
| **Stage 1:** Standard B-Tree Index | Single IP Lookup (`src_ip`) | 11.2 GB (+3.4GB idx) | `0.340 s` | 113x |
| **Stage 2:** Partitioning + BRIN Index | Range Scan (`timestamp` + `label`) | 7.9 GB (+80MB idx) | `0.118 s` | **325x** |
| **Stage 3:** Materialized View | Executive Dashboard Rollup | 12 MB | `0.004 s` | **9,600x** |
