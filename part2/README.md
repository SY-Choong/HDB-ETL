# HDB Resale Flat Price ETL - Part 2

This folder contains an AWS-based architecture design for operationalizing the HDB resale flat pipeline described in Part 1.

## Scope

The architecture supports:
- Batch ingestion of public data from data.gov.sg into an AWS-based private platform
- Secure transfer and storage in Amazon S3
- Data processing and transformation in a controlled compute environment
- Private connectivity for Tableau on AWS using Amazon Athena

## Assumptions
- The source data is publicly available and can be pulled on a scheduled batch cadence.
- The target storage is Amazon S3 in a private or restricted-access environment.
- The analytics team uses Tableau on AWS and connects via Amazon Athena.
- Network segmentation is implemented using private subnets, NAT gateways, and AWS PrivateLink where appropriate.

## Architecture Summary

### Data Ingestion
- An ingestion service runs on a scheduled basis to fetch the source data from data.gov.sg.
- Files are staged in an ingestion bucket and then moved to a curated/raw zone in S3.
- The design supports large files by using multipart transfer and a queue-based orchestration pattern.
- Public internet traffic is limited to the ingestion layer, while downstream processing remains inside private networking.

### Data Processing and Storage
- AWS Glue or managed ETL jobs process the raw files into curated datasets.
- Transformed data is stored in S3 partitioned by date and dataset.
- Access is restricted using IAM roles and bucket policies.

### Data Exploitation
- Tableau connects to Athena through a private networking path where possible.
- Athena queries access the curated data in S3 through AWS-managed services.
- The design minimizes exposure of the analytics endpoint to the public internet.

## Deliverables
- Architecture diagram: [hdb_architecture.png]
- This readme file with assumptions and design notes