
# Introduction

This repository contains notebooks that can be executed in Databricks to first generate many avro files and then read them to construct a single csv file.

The purpose is to compare how Azure Blob Storage and Azure Data Lake work in reading and writing scenarios from Databricks.

The avro files are simulating how the Azure Event Hubs Capture feature would store messages. An example scenario why someone would want to combine everything into a single csv file is when a data scientist wants to have data locally available for investigating it, for example, in a jupyter notebook.

# Sample results

Below sample results when generating 1000 files with 100 rows in each file. The end result is a 3.4 MB csv file with 100000 rows in it.

The cluster in Databricks was a Standard Pool Databricks 4.1 (includes Apache Spark 2.3.0, Scala 2.11) where workers were Standard DS3 v2 virtual machines.

The results are in seconds based on what is printed as the duration of the Stage in question.

## 1 worker

| Stage | Blob | Lake  |
|---------|-------|-------|
| Saving avro files | 90 | 60 |
| Listing files | 2  | 4 |
| Reading as JSON | 17 | 22 |
| Saving as CSV with coalesce(1) | 66 | 60 |
| Saving as CSV without coalesce | 17 | 20 |

## 10 workers

| Stage | Blob | Lake  |
|---------|-------|-------|
| Saving avro files | 14 | 10 |
| Listing files | 1  | 1 |
| Reading as JSON | 5 | 7 |
| Saving as CSV with coalesce(1) | 126 | 180 |
| Saving as CSV without coalesce | 5 | 7 |

## Observations

* Using coalesce really kills the performance, because all data needs to be collected onto a single worker and written from there
  * Increasing the cluster size makes the situation only worse
* Overall, the exact results varied a bit between runs so to get a more accurate results, multiple runs should be performed and results given as averages
