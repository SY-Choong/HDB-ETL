# Architecture Notes for Part 2

## Ingestion Design
- A scheduled ingestion job pulls the public data from data.gov.sg.
- The ingestion layer is placed at the edge of the platform and can use multipart transfer for large files.
- Files are stored initially in an S3 raw bucket before being processed by ETL jobs.
- Public access is limited to the ingestion step; downstream storage and compute remain inside private networking.

## Security Considerations
- Private subnets and NAT gateways are used for internal processing.
- S3 access is controlled with IAM roles and bucket policies.
- PrivateLink or equivalent private connectivity is used where possible for Tableau/Athena access.

## Scalability and Performance
- S3 is used as the durable data lake storage layer.
- AWS Glue or managed ETL jobs provide elastic compute for transformations.
- Partitioned data layouts in S3 improve query performance for Athena.

## Assumptions
- The source data is available as batch files from a public endpoint.
- The analytics team uses Tableau connected through Athena.
- The platform already has a VPC and basic network controls in place.
