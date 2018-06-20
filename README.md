
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
