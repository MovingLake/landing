---
title: "Effortlessly Load JSONs into BigQuery: Tips and Tricks for Success"
date: 2023-03-24
summary: "Learn about the three main methods with which you can load data into BigQuery"
image: "/img/json-load-bigquery.png"
slug: "effortlessly-load-jsons-into-bigquery-tips-and-tricks-for-success"
---
### Learn about the three main methods with which you can load data into BigQuery

BigQuery is a powerful cloud-based data warehouse and analytics platform that is capable of handling large volumes of data. One of the most common tasks performed in BigQuery is loading JSONs, which can be done in a few different ways. In this article, we'll explore the three primary paths for loading JSONs into BigQuery and provide tips and tricks for success.

## Batch Loading JSONs

Batch loading JSONs is the fastest method in terms of throughput, making it an efficient method for loading large files into BigQuery. However, there are some downsides to this method. For example, BigQuery autodetects the type of each field, which can lead to mismatches between the JSON and the table schema. Additionally, if something goes wrong during the transaction, the entire transaction will be rolled back. To avoid these issues, it's important to have a clear understanding of the JSON structure and the BigQuery schema, and to test your loading process thoroughly before executing the batch load.

One tip for success when batch loading JSONs into BigQuery is to use a schema generator tool. These tools can automatically create a schema based on the JSON file's structure, which can save time and reduce the risk of errors. Additionally, using a tool like Apache Beam can help you process and transform your data before it is loaded into BigQuery, which can improve performance and efficiency.

An example of batch loading in python would be:

<pre><code class="python">from google.cloud import bigquery
from google.cloud.bigquery import LoadJob
from google.cloud.exceptions import NotFound
from google.cloud.bigquery import ScalarQueryParameter, ArrayQueryParameter, QueryJobConfig
from google.oauth2 import service_account

credentials = service_account.Credentials.from_service_account_file(&quot;mycreds.json&quot;)
client = bigquery.Client(
    project=&quot;myproject&quot;, credentials=credentials
)

job_config = bigquery.LoadJobConfig()
job_config.source_format = bigquery.SourceFormat.NEWLINE_DELIMITED_JSON
job_config.autodetect = True

job = self._bq_client.load_table_from_json(
    table,
    f&quot;{project_id}.{dataset}.{table_name}&quot;,
    job_config=job_config,
)
</code></pre>

## Load JSONs by Crafting DML Queries

Crafting DML queries is another method for loading JSONs into BigQuery. This method supports transactions and rollbacks, but it is limited in the size of loads that can be handled. Crafting DML queries requires more coding, but it can be useful for handling small, specific loads that are not well-suited to batch loading. To make the most of this method, it's important to have a solid understanding of SQL and BigQuery syntax.

One tip for success when loading JSONs into BigQuery with DML queries is to use the UNNEST function. This function can be used to unnest arrays within the JSON file, which can simplify the loading process and reduce the risk of errors. Additionally, using standard SQL queries and breaking your load into smaller chunks can help improve performance and reduce the risk of data loss.

Here's an example of it:

<pre><code class="python">query_parameters = [bigquery.ScalarQueryParameter(None, bigquery.SchemaField(&quot;column1&quot;, &quot;STRING&quot;, mode=&quot;NULLABLE&quot;), &quot;val1&quot;), bigquery.ScalarQueryParameter(None, bigquery.SchemaField(&quot;column2&quot;, &quot;INTEGER&quot;, mode=&quot;NULLABLE&quot;), 2)]
job_config = bigquery.QueryJobConfig()
job_config.query_parameters = query_parameters
query = f&quot;INSERT INTO `{table_full_id}` (&quot;column1&quot;, &quot;column2&quot;) VALUES (?, ?)&quot;
bq_client.query(
    query,
    job_config=job_config,
).result()
</code></pre>

## Load JSONs with the Streaming API

The third method for loading JSONs into BigQuery is to use the streaming API. This method is the fastest in terms of latency, making it ideal for real-time analysis. However, there are some downsides to this method. When using the streaming API, data lands in a streaming buffer right away, but it does not immediately appear in the table itself. This means that any queries that change the table in any way will be blocked until the buffer is cleared. Additionally, if your data is not structured correctly, it can be challenging to use the streaming API effectively.

One tip for success when using the streaming API to load JSONs into BigQuery is to use the InsertAll method, which can be used to insert multiple rows of data at once. Additionally, setting up a buffer threshold can help you avoid blocking queries while still maintaining real-time data analysis. Finally, using the BigQuery Data Transfer Service can help you streamline your data loading process and reduce the risk of errors.

For example:

<pre><code class="python">table = [
  {
    &quot;column1&quot;: &quot;value1&quot;,
    &quot;column2&quot;: 2,
  },
  {
    &quot;column1&quot;: &quot;value2&quot;,
    &quot;column2&quot;: 3,
  }
]
bq_client.insert_rows_json(table_full_id, table)
</code></pre>

In conclusion, loading JSONs into BigQuery can be accomplished in several ways, each with its pros and cons. Whether you choose batch loading, crafting DML queries, or loading with the streaming API, it's important to understand the benefits and drawbacks of each method and to use best practices to ensure success. By following these tips and tricks, you can effortlessly load JSONs into
