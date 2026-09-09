# SAHO Dataset Builder

[![Python Version](https://img.shields.io/badge/python-3.8+-blue.svg)](https://python.org)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Code Style](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

Build a structured dataset from **South African History Online** (SAHO) using the site's own sitemap and embedded **Schema.org JSON-LD** data—no messy HTML parsing required!

##  Overview

This tool extracts and structures historical content from SAHO, covering:
-  **Biographies** - Notable South African figures
-  **Articles** - Historical articles and features
-  **Archive Records** - Archival materials and documents
-  **Places** - Historical locations and landmarks
-  **Historical Events** - Key moments in South African history

Instead of fragile HTML scraping, this project leverages SAHO's existing structured data (JSON-LD) to build clean, queryable datasets.

##  Features

- **Smart Extraction** - Uses Schema.org JSON-LD embedded in pages
- **Complete Coverage** - Follows official sitemap for comprehensive collection
- **Rich Metadata** - Captures titles, dates, authors, descriptions, geo-coordinates
- **Full Text Preservation** - Stores complete body text for NLP and analysis
- **Structured Output** - Clean CSV format for easy analysis
- **Minimal Dependencies** - Lightweight and fast

## Dataset Structure

Each record includes:

| Column | Description |
|--------|-------------|
| `url` | Source URL |
| `type_label` | Content type (biography, article, archive, place) |
| `fetched_at` | Scrape timestamp |
| `schema_type` | Schema.org type (Article, Person, ArchiveComponent, Place, etc.) |
| `name` | Title / person name |
| `description` | Short meta description |
| `full_text` | The full extracted body text |
| `full_text_word_count` | Word count of `full_text` |
| `date_published` | Publication date (from JSON-LD) |
| `date_modified` | Last modified date (from JSON-LD) |
| `author` | Author information (from JSON-LD) |
| `birth_date` | Birth date (biography records only) |
| `death_date` | Death date (biography records only) |
| `keywords` | Keywords/tags (from JSON-LD) |
| `jsonld` | Raw JSON-LD block (preserves everything) |

