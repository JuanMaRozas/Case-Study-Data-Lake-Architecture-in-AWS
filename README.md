# AWS Data Lake Architecture - Case Study

## Overview

This repository present a Data Lake architecture that we built in a industrial company. It was implemented in AWS to integrate and analyze data from CRM and ERP systems. The design focused on scalability, governance, and performance, using AWS managed services to ensure cost efficiency and easy maintenance. This Single Source of Truth (SSoT) approach played a key role in developing dashboards and reports and creating a centralized information hub that drove key commercial and financial goals.

![AWS_Data_Architecture_Case_study](https://github.com/user-attachments/assets/a50243bb-dc87-4b5c-98ce-6bb2b4f308ef)

## Architecture Components

### 1. Data Source Layer
- CRM and ERP systems serve as the source of data.
- Data is extracted through APIs.

### 2. Data Extraction Layer

**AWS Lambda:**
- Performs incremental data extraction.
- Queries the CRM/ERP systems for data modified since the last extraction (using last_modified timestamps).
- Writes extracted raw data into the S3 Raw Bucket in Parquet format.
- Triggers are managed via Amazon EventBridge for scheduling.

### 3. Data Storage Layer

Amazon S3 is the core storage system for the Data Lake. The storage is divided into three buckets:
**1. Raw Bucket:**
   - Stores unprocessed data directly extracted from CRM/ERP systems.
   - Serves as the source of truth for historical and traceability purposes.
**2. Curated Bucket:**
   - Contains data that has been cleaned, transformed, and normalized by the ETL process.
**3. Analytics Bucket:**
   - Optimized for high-performance querying, reporting, and machine learning.

### 4. ETL Layer
 
 **AWS Glue:**
 - First Glue Job: Extracts raw data from the S3 raw bucket, performs schema validation and transformations, and writes curated data to the S3 curated bucket.
 - Second Glue Job: Merges and processes curated data to create aggregated or feature-engineered datasets for analytics, storing them in the S3 analytics bucket.

**AWS Glue Data Catalog:**
- Maintains metadata for all datasets in S3.
- Enables easy querying through Athena or integration with QuickSight.

### 5. Governance and Security Layer

**AWS Lake Formation:**
- Enforces fine-grained access controls to secure sensitive data.
- Governs access to S3 buckets, ensuring compliance with organizational data policies.

### 6. Monitoring and Orchestration

**AWS Step Functions:**
- Orchestrates the end-to-end data pipeline.
- Handles sequential tasks like data extraction, ETL job execution, and error handling.

**Amazon CloudWatch:**
- Monitors Lambda and Glue job execution.
- Generates alerts for failures or delays in data pipelines.

### 7. Consumption Layer

**Amazon Athena:**
- Enables serverless SQL queries directly on the S3 curated and analytics buckets.

**Amazon QuickSight:**
- Visualizes processed data for dashboards and reporting.

**Amazon SageMaker:**
- Consumes data from the S3 analytics bucket for advanced machine learning workflows.

#### Key Features
- Scalability
- Governance
- Cost Efficiency
- Flexibility
