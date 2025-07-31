# Google Cloud Data, ML and AI Services

<!--toc:start-->
- [Google Cloud Data, ML and AI Services](#google-cloud-data-ml-and-ai-services)
  - [Managed Services Google offers](#managed-services-google-offers)
    - [1. Dataproc](#1-dataproc)
    - [2. Dataflow](#2-dataflow)
    - [3. BigQuery](#3-bigquery)
  - [Dataproc](#dataproc)
    - [Key Features](#key-features)
    - [Use Cases](#use-cases)
  - [Dataproc Lab 🚀](#dataproc-lab-🚀)
    - [Enable Cloud Dataproc API](#enable-cloud-dataproc-api)
    - [Assign Storage Permission to Service Account](#assign-storage-permission-to-service-account)
    - [Create a Dataproc Cluster](#create-a-dataproc-cluster)
    - [Submit a Spark Job](#submit-a-spark-job)
    - [View Job Output](#view-job-output)
    - [Update Cluster Worker Nodes](#update-cluster-worker-nodes)
    - [Rerun the Job](#rerun-the-job)
  - [Dataproc Lab with Command Line 🚀](#dataproc-lab-with-command-line-🚀)
    - [Create a Cluster](#create-a-cluster)
    - [Submit a Job](#submit-a-job)
    - [Update a Cluster](#update-a-cluster)
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

### Key Features

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

### Enable Cloud Dataproc API

1. Go to **Navigation menu > APIs & Services > Library**.
2. Search for **Cloud Dataproc API**.
3. If not already enabled, click **Enable**.

---

### Assign Storage Permission to Service Account

1. Navigate to **IAM & Admin > IAM**.
2. Locate `compute@developer.gserviceaccount.com`.
3. Click the pencil icon, then **+ ADD ANOTHER ROLE**.
4. Select **Storage Admin** and click **Save**.

---

### Create a Dataproc Cluster

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

### Submit a Spark Job

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

### View Job Output

1. Click on the **Job ID** in the Jobs list.
2. Enable **LINE WRAP** or scroll to see the calculated value of Pi.

---

### Update Cluster Worker Nodes

1. Go to **Clusters**, and click on `example-cluster`.
2. Click **Configuration**, then **Edit**.
3. Change the **Number of Worker Nodes** to `4`.
4. Click **Save**.

---

### Rerun the Job

1. Repeat the steps in **Submit a Spark Job** using the updated cluster.

## Dataproc Lab with Command Line 🚀

### Create a Cluster

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

### Submit a Job

1. **Run Spark Job**:
   ```bash
   gcloud dataproc jobs submit spark \
     --cluster example-cluster \
     --class org.apache.spark.examples.SparkPi \
     --jars file:///usr/lib/spark/examples/jars/spark-examples.jar \
     -- 1000
   ```

---

### Update a Cluster

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

### Key Features

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

### Re-enable Dataflow API
1. Open **Cloud Console** and search for **Dataflow API**.
2. Click **Manage** > **Disable API** > Confirm **Disable** > **Enable API**.
3. Verify the task via **Check my progress**.

---

### Create BigQuery Dataset, Table, and Cloud Storage Bucket (Cloud Shell)

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

### Create BigQuery Dataset, Table, and Cloud Storage Bucket (Console)

#### Create BigQuery Dataset
1. Go to **BigQuery** > **Create dataset**.
2. Set **Dataset ID** to `taxirides` and **Location** to `us`.
3. Click **Create Dataset**.

#### Create BigQuery Table
1. Open **taxirides dataset** > **Create Table**.
2. Set **Table Name** to `realtime`.
3. Add schema (Edit as text):
   ```text
   ride_id:string,point_idx:integer,latitude:float,longitude:float,timestamp:timestamp,
   meter_reading:float,meter_increment:float,ride_status:string,passenger_count:integer
   ```
4. Click **Create Table**.

#### Create Cloud Storage Bucket
1. Navigate to **Cloud Storage** > **Buckets** > **Create Bucket**.
2. Use your **Project ID** as the bucket name.
3. Click **Create**.
4. Verify the task via **Check my progress**.

---

### Run the Pipeline
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

### Submit a Query
1. Open **BigQuery Editor**.
2. Run the query:
   ```sql
   SELECT * FROM `<your_project_id>.taxirides.realtime` LIMIT 1000;
   ```
3. Wait for results in the **Query Results** panel.
