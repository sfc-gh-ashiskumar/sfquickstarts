author: Ashish
id: kafka_working_with_iceberg_tables
summary: Through this quickstart guide, you will explore how to ingest data from kafka to Iceberg tables.
categories: Getting-Started
environments: web
status: Published 
feedback link: https://github.com/Snowflake-Labs/sfguides/issues
tags: Getting Started, Data Science, Data Engineering, Twitter 

# Snowflake Guide Template
<!-- ------------------------ -->
## Overview 
Duration: 60

In this post, we will guide you through setting up open-source Kafka on a Mac system. We'll configure the Snowflake Kafka Connector to enable Snowpipe Streaming with schema evolution. This setup allows seamless data ingestion from Kafka topics into Snowflake managed Iceberg tables, ensuring real-time data flow and handling schema changes efficiently.


### Prerequisites
- Setup Kafka on MacOS
- Download snowflake-kafka-connector-2.1.0.jar from [Snowflake kafka Connector] https://mvnrepository.com/artifact/com.snowflake/snowflake-kafka-connector/3.1.0
- A Snowflake account login with a role that has the `ACCOUNTADMIN` role. 

### What You’ll Learn 
- How to setup Kafka locally
- How to configure snowflake kafka connector for snowpipe streaming
- How easily schema evolution can be setup for Iceberg tables using snowflake kafka connector


### What You’ll Need 
- A [GitHub](https://github.com/) Account 
- [VSCode](https://code.visualstudio.com/download) Installed

### What You’ll Build 
- A streaming pipeline through kafka to snowflake iceberg table


## Set up the Kafka environment


## Step 1 Download kafka in your local mac

You can download from [here](https://kafka.apache.org/downloads)

## Step 2 Start zookeeper in new terminal

```  
cd kafka_2.13-2.8.1/bin
./zookeeper-server-start.sh ../config/zookeeper.properties
```

## Step 3 Start Kafka server in new terminal

```  
cd kafka_2.13-2.8.1/bin
./kafka-server-start.sh ../config/server.properties
```

## Step 4 Download snowflake-kafka-connector from [here](https://mvnrepository.com/artifact/com.snowflake/snowflake-kafka-connector/3.1.0)

## Step 5 Run snowflake kafka connector in distributed mode in new terminal

```
<full_path>/kafka_2.13-2.8.1/bin/connect-distributed.sh <full_path>/kafka_2.13-2.8.1/config/connect-distributed.properties
```

## Step 5 create configuration for connector

```
cd <full_path/>kafka_2.13-2.8.1/config/

vi SF_connect1.json

{
    "name":"demo",
    "config":{
    "snowflake.ingestion.method":"SNOWPIPE_STREAMING",
    "snowflake.enable.schematization":true,
    "snowflake.role.name":"SYSADMIN",
    "key.converter":"org.apache.kafka.connect.json.JsonConverter",
    "value.converter":"org.apache.kafka.connect.json.JsonConverter",
    "key.converter.schemas.enable":true,
    "value.converter.schemas.enable":true,
    "key.converter.schemas.enable":false,
    "value.converter.schemas.enable":false,
    "connector.class":"com.snowflake.kafka.connector.SnowflakeSinkConnector",
    "tasks.max":"8",
    "topics":"topic1",
    "snowflake.topic2table.map":" topic1:table1",
    "buffer.count.records":"10000",
    "buffer.flush.time":"60",
    "buffer.size.bytes":"5000000",
    "snowflake.url.name":"https://aws_cas2.snowflakecomputing.com",
    "snowflake.user.name":"user",
    "snowflake.database.name":"db",
    "snowflake.schema.name":"schema1",
    "snowflake.private.key":"MIIFLTBe"

"snowflake.private.key.passphrase":"****"
    }
}

```

## Step 6 Execute configuration

```
curl -X POST -H "Content-Type: application/json" --data @<full_path>kafka_2.13-2.8.1/config/SF_connect1.json http://localhost:8083/connectors
```

## Step 6 Run kafka producer in console mode and produce some records

```
./kafka-console-producer.sh --bootstrap-server localhost:9092 --topic topic1
{"emp_id":100,"first_name":"Keshav","last_name":"Lodhi","designation":"DataEngineer"}
{"emp_id":101,"first_name":"Ashish","kumar":"Lodhi","designation":"Solution Architect"}
{"emp_id":102,"first_name":"Anup","last_name":"moncy","designation":"Solution Architect"}
{"emp_id":102,"first_name":"Anup","last_name":"moncy","designation":"Solution Architect","company":"Snowflake"}
{"emp_id":103,"first_name":"Demo","last_name":"Demo","designation":"Solution Architect","company":"Snowflake","contact":"123456789"}
```

## Conclusion And Resources
At the end of your Snowflake Guide, always have a clear call to action (CTA). This CTA could be a link to the docs pages, links to videos on youtube, a GitHub repo link, etc. 

### What You Learned
- creating steps and setting duration
- adding code snippets
- embedding images, videos, and surveys
- importing other markdown files

### Related Resources
- [Using Snowflake Connector for Kafka with Snowpipe Streaming | Snowflake Documentation](https://docs.snowflake.com/en/user-guide/data-load-snowpipe-streaming-kafka)
