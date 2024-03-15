# AWS Glue Features

This document is just a roundup of some key features in Glue that may be relevant to your project:

AWS Glue is specifically designed for ETL job with inbuilt horizontal scaling* for ETL jobs (Spark Distributed processing), Data Catalog, Data Crawlers along with inbuilt compatibility with data formatting/storage (Apache Spark Support).

## Error Handling 

- It is recommended as a requirement to have error logging available via CloudWatch 
- You can create custom script logging (example at the side how this can be set below). You need continuous logging to enabled for this, [click here for more info](https://docs.aws.amazon.com/glue/latest/dg/monitor-continuous-logging-enable.html)
- You could or enable Apache Spark Web UI or Job Run Insights to gain greater granularity  

## Sharing states between Glue Jobs 

- You can use the AWS Glue Workflow to orchestrate this but we primarily are using AWS Step Functions to orchestrate simple pipelines, for more complex you could look at Glue workflow or Apache Airflow etc 

[More info here on AWS Glue Workflow and sharing states](https://docs.aws.amazon.com/whitepapers/latest/aws-glue-best-practices-build-secure-data-pipeline/building-a-reliable-data-pipeline.html#sharing-state-using-aws-glue-workflow-properties)

## Scaling 

If you are looking for optimisations in Glue handling large amounts of data (separate to performance improvements in Kafka/Kinesis etc), two key ways recommended by AWS are:


- Increase Spark Partitions: Multiple partitions can execute transformations on multiple partitions in parallel (check out spark by examples for a good article on this: https://sparkbyexamples.com/spark/spark-partitioning-understanding/)

- Add more workers - This should be use with caution as without proper optimisation then a limited amount of workers could be doing lot of the work, mean the other workers are idle. 

- Updating to the latest Glue Version

## Job Bookmarks

Using Job Bookmarks will keep track of processed data and will process new data since last checkpoint


There are also options to process data without updating the bookmark state (using options job-bookmark-from & job-bookmark-to) plus specify bookmark keys (such as specific columns) 

[Click here for more info](https://docs.aws.amazon.com/glue/latest/dg/monitor-continuations.html)

## AWS Glue Data Catalog & AWS Glue Crawlers 

The AWS Glue Data Catalog contains references to data that is used as sources and targets of your extract, transform, and load (ETL) jobs in AWS Glue. Information in the Data Catalog is stored as metadata tables, where each table specifies a single data store. 

AWS Glue Data Crawler update the AWS Glue Data Catalog. Once these crawlers update the catalog, then the ETL jobs use the updated reference in the catalog tables to then read and update data
They can crawl native, JDBC or MongoDB clients for data 

It has many features but one used in a previous project is to use AWS S3 events to trigger the crawler to improve recrawl time


## Testing

Testing can be achieved in many different ways but popular ways are as so:


- Manual testing, running the glue jobs on a test environment 
- AWS Glue Interactive Sessions
- Jupyter Notebooks 
- Docker Containers 
- Local testing with AWS Glue ETL library (you need to use the Apache Maven build system) 

## links
https://docs.aws.amazon.com/whitepapers/latest/aws-glue-best-practices-build-secure-data-pipeline/building-a-reliable-data-pipeline.html

https://docs.aws.amazon.com/prescriptive-guidance/latest/tuning-aws-glue-for-apache-spark/performance-tuning-strategies.html

https://docs.aws.amazon.com/glue/latest/dg/monitor-continuations.html

