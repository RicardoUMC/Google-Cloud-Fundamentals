# Google Cloud Data, ML and AI Services

<!--toc:start-->
- [Google Cloud Data, ML and AI Services](#google-cloud-data-ml-and-ai-services)
  - [Managed Services Google offers](#managed-services-google-offers)
    - [1. Dataproc](#1-dataproc)
    - [2. Dataflow](#2-dataflow)
    - [3. BigQuery](#3-bigquery)
  - [Dataproc](#dataproc)
    - [Dataproc Features](#dataproc-features)
    - [Use Cases](#use-cases)
  - [Dataproc Lab 🚀](#dataproc-lab-🚀)
    - [1. Enable Cloud Dataproc API](#1-enable-cloud-dataproc-api)
    - [2. Assign Storage Permission to Service Account](#2-assign-storage-permission-to-service-account)
    - [3. Create a Dataproc Cluster](#3-create-a-dataproc-cluster)
    - [4. Submit a Spark Job](#4-submit-a-spark-job)
    - [5. View Job Output](#5-view-job-output)
    - [6. Update Cluster Worker Nodes](#6-update-cluster-worker-nodes)
    - [7. Rerun the Job](#7-rerun-the-job)
  - [Dataproc Lab with Command Line 🚀](#dataproc-lab-with-command-line-🚀)
    - [1. Create a Cluster](#1-create-a-cluster)
    - [2. Submit a Job](#2-submit-a-job)
    - [3. Update a Cluster](#3-update-a-cluster)
  - [Build extract, transform, and load (ETL) pipelines with Dataproc](#build-extract-transform-and-load-etl-pipelines-with-dataproc)
    - [ETL Features](#etl-features)
    - [Integration](#integration)
    - [Challenges in ETL Pipelines](#challenges-in-etl-pipelines)
  - [Templates Lab 🚀](#templates-lab-🚀)
    - [1. Re-enable Dataflow API](#1-re-enable-dataflow-api)
    - [2. Create BigQuery Dataset, Table, and Cloud Storage Bucket (Cloud Shell)](#2-create-bigquery-dataset-table-and-cloud-storage-bucket-cloud-shell)
      - [Create BigQuery Dataset](#create-bigquery-dataset)
      - [Create BigQuery Table](#create-bigquery-table)
      - [Create Cloud Storage Bucket](#create-cloud-storage-bucket)
    - [3. Create BigQuery Dataset, Table, and Cloud Storage Bucket (Console)](#3-create-bigquery-dataset-table-and-cloud-storage-bucket-console)
      - [Create BigQuery Dataset (Console)](#create-bigquery-dataset-console)
      - [Create BigQuery Table (Console)](#create-bigquery-table-console)
      - [Create Cloud Storage Bucket (Console)](#create-cloud-storage-bucket-console)
    - [4. Run the Pipeline](#4-run-the-pipeline)
    - [5. Submit a Query](#5-submit-a-query)
  - [Dataflow Python Lab 🚀](#dataflow-python-lab-🚀)
    - [1. Create a Cloud Storage Bucket](#1-create-a-cloud-storage-bucket)
      - [Verify:](#verify)
    - [2. Install Apache Beam SDK for Python](#2-install-apache-beam-sdk-for-python)
    - [3. Run a Dataflow Pipeline Remotely](#3-run-a-dataflow-pipeline-remotely)
    - [4. Verify Dataflow Job Success](#4-verify-dataflow-job-success)
      - [Verify:](#verify)
<!--toc:end-->

## Managed Services Google offers

### 1. Dataproc

- Ideal for companies using Apache Hadoop and Apache Spark.
- Enables running open-source software on Google Cloud.

---

### 2. Dataflow

- Designed for large-scale batch processing and long-running streaming.
- Handles structured and unstructured data efficiently.

---

### 3. BigQuery

- A data analytics service optimized for rapid queries over petabyte-scale datasets.
- Supports fast SQL-based analysis on unstructured data.

## Dataproc

Dataproc is a managed Spark and Hadoop service on Google Cloud, ideal for running open-source tools for batch processing, querying, streaming, and machine learning.

---

### Dataproc Features

- **Cost-Effective**: Priced at 1 cent per virtual CPU per cluster per hour. Supports preemptible instances for lower costs.
- **Fast and Scalable**: Clusters start, scale, and shut down in under 90 seconds. Supports various VM types, disk sizes, and networking options.
- **Open-Source Ecosystem**: Native support for Spark, Hadoop, Pig, and Hive. No need to learn new tools or APIs.
- **Fully Managed**: Easy cluster and job management via Google Cloud Console, SDK, or REST API.
- **Image Versioning**: Switch between different versions of Spark, Hadoop, and more.
- **Built-in Integration**: Seamlessly integrates with Cloud Storage, BigQuery, Bigtable, Cloud Logging, and Cloud Monitoring.

---

### Use Cases

1. **Log Data Processing**: Process 50GB of daily log data stored in Cloud Storage with Dataproc clusters. Clusters run only when needed, reducing costs.
2. **Scalable Spark Shell**: Analysts use Spark Shell while IT scales clusters with Dataproc to handle increased demand, ensuring fast computations.
3. **Machine Learning**: Run Spark MLlib for large datasets. Create customizable clusters quickly and monitor workflows with Cloud Logging and Monitoring.

Dataproc simplifies big data processing, reduces costs, and integrates seamlessly with Google Cloud products.

## Dataproc Lab 🚀

### 1. Enable Cloud Dataproc API

1. Go to **Navigation menu > APIs & Services > Library**.
2. Search for **Cloud Dataproc API**.
3. If not already enabled, click **Enable**.

---

### 2. Assign Storage Permission to Service Account

1. Navigate to **IAM & Admin > IAM**.
2. Locate `compute@developer.gserviceaccount.com`.
3. Click the pencil icon, then **+ ADD ANOTHER ROLE**.
4. Select **Storage Admin** and click **Save**.

---

### 3. Create a Dataproc Cluster

1. Go to **Navigation menu > View all products > Dataproc > Clusters**, then click **Create cluster**.
2. Set the following values:
   - **Name**: `example-cluster`
   - **Region**: `____`
   - **Zone**: `____`
   - **Machine Series (Manager Node)**: E2
   - **Primary Disk Type (Manager Node)**: Standard Persistent Disk
   - **Machine Type (Manager Node)**: e2-standard-2
   - **Primary Disk Size (Manager Node)**: 30 GB
   - **Number of Worker Nodes**: 2
   - **Primary Disk Type (Worker Node)**: Standard Persistent Disk
   - **Machine Series (Worker Nodes)**: E2
   - **Machine Type (Worker Nodes)**: e2-standard-2
   - **Primary Disk Size (Worker Nodes)**: 30 GB
3. Deselect **Internal IP only**.
4. Click **Create**. Wait until the cluster status changes to **Running**.

---

### 4. Submit a Spark Job

1. Navigate to **Jobs**, then click **Submit job**.
2. Set the following values:
   - **Region**: `____`
   - **Cluster**: `example-cluster`
   - **Job type**: Spark
   - **Main class or jar**: `org.apache.spark.examples.SparkPi`
   - **Jar files**: `file:///usr/lib/spark/examples/jars/spark-examples.jar`
   - **Arguments**: `1000`
3. Click **Submit**.

---

### 5. View Job Output

1. Click on the **Job ID** in the Jobs list.
2. Enable **LINE WRAP** or scroll to see the calculated value of Pi.

---

### 6. Update Cluster Worker Nodes

1. Go to **Clusters**, and click on `example-cluster`.
2. Click **Configuration**, then **Edit**.
3. Change the **Number of Worker Nodes** to `4`.
4. Click **Save**.

---

### 7. Rerun the Job

1. Repeat the steps in **Submit a Spark Job** using the updated cluster.

## Dataproc Lab with Command Line 🚀

### 1. Create a Cluster

1. **Set the Region**:
   ```bash
   gcloud config set dataproc/region REGION
   ```

2. **Set up Project**:
   ```bash
   PROJECT_ID=$(gcloud config get-value project)
   gcloud config set project $PROJECT_ID
   PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format='value(projectNumber)')
   ```

3. **Assign Storage Permissions**:
   ```bash
   gcloud projects add-iam-policy-binding $PROJECT_ID \
     --member=serviceAccount:$PROJECT_NUMBER-compute@developer.gserviceaccount.com \
     --role=roles/storage.admin
   ```

4. **Enable Private Google Access**:
   ```bash
   gcloud compute networks subnets update default \
     --region=REGION --enable-private-ip-google-access
   ```

5. **Create Cluster**:
   ```bash
   gcloud dataproc clusters create example-cluster \
     --worker-boot-disk-size 500 \
     --worker-machine-type=e2-standard-4 \
     --master-machine-type=e2-standard-4
   ```

---

### 2. Submit a Job

1. **Run Spark Job**:
   ```bash
   gcloud dataproc jobs submit spark \
     --cluster example-cluster \
     --class org.apache.spark.examples.SparkPi \
     --jars file:///usr/lib/spark/examples/jars/spark-examples.jar \
     -- 1000
   ```

---

### 3. Update a Cluster

1. **Scale Workers to 4 Nodes**:
   ```bash
   gcloud dataproc clusters update example-cluster --num-workers 4
   ```

2. **Scale Workers to 2 Nodes** (if needed):
   ```bash
   gcloud dataproc clusters update example-cluster --num-workers 2
   ```

## Build extract, transform, and load (ETL) pipelines with Dataproc

Dataflow is a Google Cloud managed service designed for large-scale batch and streaming data processing. It automates the creation of pipelines to perform **Extract, Transform, and Load (ETL)** operations efficiently.

---

### ETL Features

- **Stream & Batch Processing**: Supports both streaming and batch data processing.
- **Fully Managed**: Automates resource management, scaling, and performance optimization.
- **Fault Tolerance**: Ensures consistent and reliable execution across varying data sizes and complexities.
- **Real-Time Insights**: Provides pipeline statistics, throughput, lag, and worker logs in near real-time.

---

### Integration

Dataflow seamlessly integrates with:
- **Cloud Storage**
- **Pub/Sub**
- **Datastore**
- **Bigtable**
- **BigQuery**

---

### Challenges in ETL Pipelines

- Ensuring compatibility of pipeline code with both batch and streaming data.
- Supporting transformations, aggregations, windowing, and handling late-arriving data.
- Leveraging existing templates or solutions.

Dataflow simplifies these challenges by offering a fully managed, scalable, and fault-tolerant platform for building ETL pipelines.

## Templates Lab 🚀

### 1. Re-enable Dataflow API
1. Open **Cloud Console** and search for **Dataflow API**.
2. Click **Manage** > **Disable API** > Confirm **Disable** > **Enable API**.
3. Verify the task via **Check my progress**.

---

### 2. Create BigQuery Dataset, Table, and Cloud Storage Bucket (Cloud Shell)

#### Create BigQuery Dataset
1. Run:
   ```bash
   bq mk taxirides
   ```

#### Create BigQuery Table
1. Run:
   ```bash
   bq mk \
   --time_partitioning_field timestamp \
   --schema ride_id:string,point_idx:integer,latitude:float,longitude:float,\
   timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,\
   passenger_count:integer -t taxirides.realtime
   ```

#### Create Cloud Storage Bucket
1. Set bucket name:
   ```bash
   export BUCKET_NAME=<your_project_id>
   ```
2. Create bucket:
   ```bash
   gsutil mb gs://$BUCKET_NAME/
   ```
3. Verify the task via **Check my progress**.

---

### 3. Create BigQuery Dataset, Table, and Cloud Storage Bucket (Console)

#### Create BigQuery Dataset (Console)
1. Go to **BigQuery** > **Create dataset**.
2. Set **Dataset ID** to `taxirides` and **Location** to `us`.
3. Click **Create Dataset**.

#### Create BigQuery Table (Console)
1. Open **taxirides dataset** > **Create Table**.
2. Set **Table Name** to `realtime`.
3. Add schema (Edit as text):
   ```text
   ride_id:string,point_idx:integer,latitude:float,longitude:float,timestamp:timestamp,
   meter_reading:float,meter_increment:float,ride_status:string,passenger_count:integer
   ```
4. Click **Create Table**.

#### Create Cloud Storage Bucket (Console)
1. Navigate to **Cloud Storage** > **Buckets** > **Create Bucket**.
2. Use your **Project ID** as the bucket name.
3. Click **Create**.
4. Verify the task via **Check my progress**.

---

### 4. Run the Pipeline
1. Deploy the Dataflow template:
   ```bash
   gcloud dataflow jobs run iotflow \
   --gcs-location gs://dataflow-templates-us-east4/latest/PubSub_to_BigQuery \
   --region us-east4 \
   --worker-machine-type e2-medium \
   --staging-location gs://<your_bucket_name>/temp \
   --parameters inputTopic=projects/pubsub-public-data/topics/taxirides-realtime,outputTableSpec=<your_project_id>:taxirides.realtime
   ```
2. Monitor the job in **Dataflow > Jobs**.
3. Verify the task via **Check my progress**.

---

### 5. Submit a Query
1. Open **BigQuery Editor**.
2. Run the query:
   ```sql
   SELECT * FROM `<your_project_id>.taxirides.realtime` LIMIT 1000;
   ```
3. Wait for results in the **Query Results** panel.

## Dataflow Python Lab 🚀

### 1. Create a Cloud Storage Bucket

1. Navigate to **Cloud Storage > Buckets** in the Google Cloud Console.
2. Click **Create bucket** and specify the following:
   - **Name**: Use a unique name like `qwiklabs-gcp-04-241a6f9e39c9-bucket`.
   - **Location type**: Multi-region.
   - **Location**: `us`.
3. Click **Create**.
4. Confirm if prompted to prevent public access.

---

### 2. Install Apache Beam SDK for Python

1. Run a Python 3.9 Docker container:
   ```bash
   docker run -it -e DEVSHELL_PROJECT_ID=$DEVSHELL_PROJECT_ID python:3.9 /bin/bash
   ```
2. Install Apache Beam SDK:
   ```bash
   pip install 'apache-beam[gcp]'==2.42.0
   ```
3. Run the `wordcount.py` example locally:
   ```bash
   python -m apache_beam.examples.wordcount --output OUTPUT_FILE
   ```
4. View the output:
   ```bash
   ls
   cat <OUTPUT_FILE>
   ```

---

### 3. Run a Dataflow Pipeline Remotely

1. Set the bucket environment variable:
   ```bash
   BUCKET=gs://<your_bucket_name>
   ```
2. Run the `wordcount.py` example:
   ```bash
   python -m apache_beam.examples.wordcount --project $DEVSHELL_PROJECT_ID \
     --runner DataflowRunner \
     --staging_location $BUCKET/staging \
     --temp_location $BUCKET/temp \
     --output $BUCKET/results/output \
     --region us-central1
   ```

---

### 4. Verify Dataflow Job Success

1. Go to **Navigation menu > Dataflow**.
2. Ensure the job status is **Running**, then wait until it changes to **Succeeded**.
3. Explore the output in **Cloud Storage > Buckets**:
   - Open your bucket and navigate to the `results` folder.
   - View the output files to see word counts.

