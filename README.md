
# Introduction

This repository contains notebooks that can be executed in Databricks to first generate many avro files and then read them to construct a single csv file.

The purpose is to compare how Azure Blob Storage and Azure Data Lake work in reading and writing scenarios from Databricks.

The avro files are simulating how the Azure Event Hubs Capture feature would store messages. An example scenario why someone would want to combine everything into a single csv file is when a data scientist wants to have data locally available for investigating it, for example, in a jupyter notebook.

# Sample results

Below sample results when generating 1000 files with 100 rows in each file. The end result is a 3.4 MB csv file with 100000 rows in it.

The cluster in Databricks was a Serverless Pool where workers were Standard DS3 v2 virtual machines. The results are seconds based on how long Databricks reports the command took.

## 1-2 workers

| Storage | Write | Read  |
|---------|-------|-------|
| Blob    | 112.2 | 210.0 |
| Lake    | 88.2  | 268.2 |

## 10-11 workers

| Storage | Write | Read  |
|---------|-------|-------|
| Blob    | 108.6 | 168.6 |
| Lake    | 25.0  | 220.2 |

\* Without using `coalesce(1)` to combine onto a single csv file, the read is a lot faster (55.5 with Blob and 59.2 with Lake), but the result is around 40 individual csv files that would need to be combined with an additional step.

** Without any kind of write and just collecting the content into a Spark DataFrame, the results were 35.8 with Blob and 43.7 with Lake.

*** Overall, the exact results varied a bit between runs so to get a more accurate results, multiple runs should be performed and results given as averages