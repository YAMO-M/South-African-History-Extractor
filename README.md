# SAHO Dataset Builder

[![Python Version](https://img.shields.io/badge/python-3.8+-blue.svg)](https://python.org)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

Build a structured dataset from **South African History Online** (SAHO) using the site's own listing pages and embedded **Schema.org JSON-LD** data—no messy HTML parsing required!

## Overview

This tool extracts and structures historical content from SAHO, covering:

- **Biographies** — Notable South African figures
- **Articles** — Historical articles and features
- **Archive Records** — Archival materials and documents
- **Places** — Historical locations and landmarks
- **Historical Events** — Key moments in South African history

Instead of fragile HTML scraping, this project leverages SAHO's existing structured data (JSON-LD) to build clean, queryable datasets.

## Features

- **Smart Extraction** — Uses Schema.org JSON-LD embedded in every page
- **Complete Coverage** — Paginates through SAHO's listing pages for comprehensive collection
- **Rich Metadata** — Captures titles, dates, authors, descriptions, keywords, and geo-coordinates
- **Full Text Preservation** — Stores the complete article body for NLP and analysis
- **Structured Output** — Clean JSONL with optional flattened CSV export
- **Resilient Crawling** — Graceful error handling, resume support, and respectful rate limiting
- **Quality Cleaning** — Deduplication (exact + near-duplicate), stub filtering, language detection, and boilerplate flagging
- **Minimal Dependencies** — Lightweight and fast

## Dataset Structure

Each record is stored as **one JSON object per line** (JSONL) with the following fields:

| Field | Description |
|-------|-------------|
| `_source_url` | Source URL of the page |
| `_type_label` | Content type (`biography`, `article`, `archive`, `place`, `event`) |
| `_fetched_at` | Scrape timestamp (ISO 8601) |
| `jsonld` | Raw Schema.org JSON-LD block(s) — preserves everything |
| `full_text` | The full extracted body text |
| `full_text_word_count` | Word count of `full_text` |
| `document_hash` | SHA-256 hash of `full_text` (used for deduplication) |

### Flattened CSV Export

When `--csv` is passed, records are flattened into a cleaner tabular form:

| Column | Description |
|--------|-------------|
| `url` | Source URL |
| `type_label` | Content type |
| `retrieval_date` | Scrape timestamp |
| `document_hash` | SHA-256 content hash |
| `schema_type` | Schema.org type (`Article`, `Person`, `ArchiveComponent`, `Place`, etc.) |
| `name` | Title / person name |
| `description` | Short meta description |
| `date_published` | Publication date (from JSON-LD) |
| `date_modified` | Last modified date (from JSON-LD) |
| `author` | Author information (from JSON-LD) |
| `birth_date` | Birth date (*biography records only*) |
| `death_date` | Death date (*biography records only*) |
| `keywords` | Keywords / tags (from JSON-LD) |
| `full_text` | The full extracted body text |
| `full_text_word_count` | Word count of `full_text` |

### Output Layout
saho_dataset/

├── biography.jsonl              # raw scrape output

├── article.jsonl

├── archive.jsonl

├── place.jsonl

├── event.jsonl

├── biography.csv                # optional flattened CSV

├── article.csv

└── clean/

    ├── manifest_biography.jsonl # cleaned + deduped final dataset
    
    ├── manifest_article.jsonl
    
    ├── dropped_biography.jsonl  # audit log of removed records
    
    └── dropped_article.jsonl






### Installation 
git clone https://github.com/yourusername/South-African-History-Extractor.git
cd South-African-History-Extractor
pip install -r requirements.txt

### Usage 
 Small test run — scrape 50 biographies and articles
python saho_pipeline.py --types biography article --limit 50 --out ./saho_dataset

 Full crawl of all content types
python saho_pipeline.py --types biography article archive place event --out ./saho_dataset

 Export flattened CSVs alongside the JSONL
python saho_pipeline.py --types biography article --csv --out ./saho_dataset

 Re-clean existing raw JSONL without re-scraping
python saho_pipeline.py --types biography article --skip-scrape --out ./saho_dataset



### Disclaimer
This project is not affiliated with or endorsed by South African History Online. Please respect SAHO's terms of use, robots directives, and rate limits when running the crawler. Default crawl delay is set to 1.1 seconds.
