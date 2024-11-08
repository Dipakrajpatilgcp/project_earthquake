# Seismic Data ETL Pipeline

## Table of Contents
1. [Project Overview](#project-overview)
2. [Features](#features)
3. [Architecture](#architecture)
4. [Directory Structure](#directory-structure)
5. [Modules](#modules)
   - [Config](#config)
   - [Seismic Data Pipeline](#seismic-data-pipeline)
   - [Historical and Daily ETL Scripts](#historical-and-daily-etl-scripts)
6. [Setup and Installation](#setup-and-installation)
7. [Usage](#usage)
8. [Future Enhancements](#future-enhancements)
9. [Contributing](#contributing)

## Project Overview

This project is an ETL (Extract, Transform, Load) pipeline for seismic data. The pipeline fetches data from the USGS Earthquake API, processes and transforms it using PySpark, stores raw and transformed data in Google Cloud Storage (GCS), and loads the final dataset into Google BigQuery for analytics and reporting.

The pipeline consists of two main ETL workflows:
- **Historical ETL**: Processes seismic data from the past month.
- **Daily ETL**: Processes seismic data from the last 24 hours.

The project is designed for scalability, automation, and optimal resource management using cloud-native technologies.

## Features

- **Automated Data Ingestion**: Fetches seismic data in JSON format from USGS APIs.
- **Data Transformation**: Utilizes PySpark for flattening, filtering, and transforming data to a structured format.
- **Cloud Storage Management**: Stores both raw and transformed data in Google Cloud Storage.
- **BigQuery Integration**: Loads final transformed data into BigQuery tables, enabling easy querying and analytics.
- **Modular Code Design**: Well-organized modules for easy maintenance and extensibility.
- **Daily and Monthly Scheduling**: Supports historical data backfills and daily data ingestion.

## Architecture

The pipeline architecture consists of the following stages:
1. **Data Ingestion**: Fetches seismic data from the USGS API (different endpoints for daily and historical data).
2. **Data Storage (Landing Layer)**: Stores raw data in JSON format in GCS in a landing bucket.
3. **Data Transformation (Silver Layer)**: Transforms data with PySpark, storing the transformed Parquet files back to GCS in the Silver layer.
4. **Data Loading (Gold Layer)**: Loads transformed data into BigQuery tables for querying.

The modular design leverages PySpark for distributed processing and Google Cloud services for storage and database management, ensuring data is available for reporting and analytics.

## Directory Structure

'''
.
├── seismic_data_pipeline/
│   ├── seismic_data_pipeline.py     # Core pipeline code with ETL modules
│   ├── config.py                    # Configuration file for API URLs, GCS, BigQuery
│   └── README.md                    # Project documentation
├── historical_etl.py                # Main script for historical data ETL
├── daily_etl.py                     # Main script for daily data ETL
├── requirements.txt                 # Python dependencies
'''

## Modules

### Config
The `config.py` module stores all project configurations, including:
- **API Endpoints**:
  - `SOURCE_API_URL_HISTORICAL`: URL for the historical seismic data API.
  - `SOURCE_API_URL_DAILY`: URL for the daily seismic data API.
- **Google Cloud Configurations**:
  - `GCS_BUCKET`: Google Cloud Storage bucket for storing data.
  - `PROJECT_ID`, `DATASET_ID`, `TABLE_ID`: BigQuery configurations for project, dataset, and table.
- **File Paths**:
  - `PYSPARK_LANDING_LOCATION`: Path for landing layer data in GCS.
  - `PYSPARK_SILVER_LAYER`: Path for Silver layer in GCS.

### Seismic Data Pipeline
The `seismic_data_pipeline.py` module includes key classes and functions for each pipeline stage:
- **SeismicDataFetcher**:
  - Fetches seismic data from a specified API URL.
  - Utilizes requests to retrieve data and validates responses.
- **CloudStorageManager**:
  - Uploads JSON data to GCS in the landing layer.
  - Downloads data from GCS for transformation and handles storage in the Silver layer.
  - Reads Silver layer data in preparation for BigQuery.
- **SeismicDataTransformation**:
  - Transforms raw JSON data to a structured format using PySpark.
  - Flattens nested structures, converts timestamps, and applies filtering and transformation rules.
- **BigQueryUploader**:
  - Transforms data for BigQuery schema compatibility.
  - Uploads the transformed data from the Silver layer to BigQuery.

### Historical and Daily ETL Scripts
- **Historical ETL (`historical_etl.py`)**: This module runs the ETL pipeline for historical seismic data, configured to process the last month's data from the `SOURCE_API_URL_HISTORICAL` API endpoint.
- **Daily ETL (`daily_etl.py`)**: This module runs the ETL pipeline for daily seismic data, fetching data from the `SOURCE_API_URL_DAILY` endpoint and processing it into BigQuery.

## Setup and Installation

### Prerequisites
- Python 3.8 or higher
- Google Cloud SDK
- PySpark
- Google Cloud Storage and BigQuery configured with appropriate IAM roles

### Installation
1. Clone the repository:
   git clone https://github.com/your-username/seismic-data-pipeline.git
   cd seismic-data-pipeline
