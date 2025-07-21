# Google Cloud Infrastructure

## Option Storage

Google Cloud offers relational and non-relational databases and worldwide object storage. Provides managed storage and database services that are scalable, reliable, and easy to operate. These include: 

* Cloud Storage
* Cloud SQL
* Spanner
* Firestore
* Bigtable

Three common cloud storage use cases:

1. Content storage: Images or videos need to be served to users wherever they are.
2. Data analytics and general compute: Users can expose their data to analytcs tools, like the analytics stacl of products that Google Cloud offers, and do things like genomic sequencing or IoT data analysis.
3. Backup and archival storage: Users can save storage costs by migrating infrequently accessed content to cheaper cloud storage options.

For users with databases, Google’s first priority is to help them migrate existing databases to the cloud and move them to the right service.

### Structured Data

Transactional workload:

* **Cloud SQL:** For local/regional scalability (SQL).
* **Spanner:** Global scalability (SQL).
* **Firestore:** When the transactional data will be accessed without SQL.

Analytical workload:

* **BigQuery:** Google's data warehouse solution, lets you analyze petabyte-scale datasets (SQL).
* **Bigtable:** Best for real-time, high-throughput applications that require only millisecond latency (NoSQL).

### Object Storage

It is a computer data storage architecture that manages data as "objects" and not as a file and folder hierarchy (file storage), or as chunks of a disk (block storage).

These objects are stored in a packaged format that contains the binary form of the actual data itsef, relevant associated metadata (sush as date created, author, resource type, and permissions), and a globally unique identifier.

These unique keys are in the form of **URLs**, which means object storage interacts well with web technologies.

#### Four basic Cloud Storage classes:

* Standard Storage: Hot data ($)
* Nearline Storage: One per month ($$)
* Nearline Storage: Once every 90 days ($$$)
* Archive Storage: Once a year ($$$$)

All these include unlimited storage with no minimum object size requirement, Worldwide accessibility and locations, Low latency and high durability, A
uniform experience, which extends to security, tools, and APIs, and geo-redundancy if data is stored in a multi-region or dual-region.

#### Immutability

The storage objects offered by Cloud Storage are “immutable,” which means that you don’t edit them, but instead a new version is created with every change made.

Administrators can either allow each new version to completely overwrite the older one or keep track of each change made to a particular object by enabling “versioning” within a bucket.

* If you choose to use versioning, Cloud Storage will keep a detailed history of modifications -- that is, overwrites or deletes -- of all objects contained in that bucket.

* If you don’t turn on object versioning, by default new versions will always overwrite older versions.
With object versioning enabled, you can list the archived versions of an object, restore an object to an older state, or permanently delete a version of an object, as needed.

## Creating a bucket

**Bucket naming rules**

* Do not include sensitive information in the bucket name, because the bucket namespace is global and publicly visible.
* Bucket names must contain only lowercase letters, numbers, dashes (-), underscores (_), and dots (.). Names containing dots require domain verification.
* Bucket names must start and end with a number or letter.
* Bucket names must contain 3 to 63 characters. Names containing dots can contain up to 222 characters, but each dot-separated component can be no longer than 63 characters.
* Bucket names cannot be represented as an IP address in dotted-decimal notation (for example, 192.168.5.4).
* Bucket names cannot begin with the "goog" prefix.
* Bucket names cannot contain "google" or close misspellings of "google".
* Also, for DNS compliance and future compatibility, you should not use underscores (_) or have a period adjacent to another period or dash. For example, ".." or "-." or ".-" are not valid in DNS names.

### Make bucket command:

```bash
gcloud storage buckets create gs://<YOUR-BUCKET-NAME>
```

### Upload an object into your bucket:

1. To download this image (ada.jpg) into your bucket, enter this command into Cloud Shell:

```bash
curl https://upload.wikimedia.org/wikipedia/commons/thumb/a/a4/Ada_Lovelace_portrait.jpg/800px-Ada_Lovelace_portrait.jpg --output ada.jpg
```

2. Use the `gcloud storage cp` command to upload the image from the location where you saved it to the bucket you created:

```bash
gcloud storage cp ada.jpg gs://YOUR-BUCKET-NAME
```

3. Now remove the downloaded image:

```bash
rm ada.jpg
```

### Download an object from your bucket:

```bash
gcloud storage cp -r gs://YOUR-BUCKET-NAME/ada.jpg .
```

### Copy an object to a folder in the bucket:

```bash
gcloud storage cp gs://YOUR-BUCKET-NAME/ada.jpg gs://YOUR-BUCKET-NAME/image-folder/
```

### List contents of a bucket or folder:

```bash
gcloud storage ls gs://YOUR-BUCKET-NAME
```

### List details for an object:

```bash
gcloud storage ls -l gs://YOUR-BUCKET-NAME/ada.jpg
```

### Make your object publicly accessible:

```bash
gsutil acl ch -u AllUsers:R gs://YOUR-BUCKET-NAME/ada.jpg
```

### Remove public access:

```bash
gsutil acl ch -d AllUsers gs://YOUR-BUCKET-NAME/ada.jpg
```

### Delete objects:

```bash
gcloud storage rm gs://YOUR-BUCKET-NAME/ada.jpg
```

## Cloud SQL

Cloud SQL is a fully managed database service that makes it easy to set up, maintain, manage, and administer relational databases on Google Cloud. It supports MySQL, PostgreSQL, and SQL Server.

It doesn't require any additional software or hardware, and it is designed to be easy to use, with a simple web interface and command-line tools.

### Reasons to use Cloud SQL 

* Doesn't require any software or maintenance.
* Can scale up to 96 processor cores, 624 GB of memory and 64 TB of storage.
* Supports automatic replication scenarios.
* Supports managed backups - The Cost of an instance covers 7 backups.
* Encrypts customer data when on Google's internal networks and when stored in database tables, temporary files, and backups.
* Includes a network firewall.

### Create a Cloud SQL instance

1. From the Navigation menu click on SQL.

2. Click Create Instance.

3. On the Choose your database engine panel of the Create an instance page, click Choose MySQL.

4. For Choose a Cloud SQL edition, select Enterprise edition.

5. For Preset choose Development (4 vCPU, 16 GB RAM, 100 GB Storage, Single zone).

> [!WARNING]
> If you choose a preset larger than Development, your project will be flagged and your lab will be terminated.

6. Select the database version as MySQL 8.

7. Enter Instance ID as `myinstance`.

8. In the password field click on the Generate link and the eye icon to see the password. Save the password to use in the next section.

9. In Choose region and zonal availability section, region is already set to us-east1

10. Set the Multi zones (Highly available) > Primary Zone field as us-east1-c.

11. Click CREATE INSTANCE.

It might take a few minutes for the instance to be created. Once it is, you will see a green checkmark next to the instance name.

12. Click on the Cloud SQL instance. The SQL Overview page opens.

### Connect to your instance using the mysql client in Cloud Shell

1. In the Cloud Console, click the Cloud Shell icon in the upper right corner.

2. Click Continue.

3. At the Cloud Shell prompt, connect to your Cloud SQL instance by running the following:

```bash
gcloud sql connect myinstance --user=root
```

Click **Authorize**.

4. Enter your root password when prompted. Note: The cursor will not move.

5. Press the Enter key when you're done typing.

You should now see the `mysql` prompt.

### Create a database and upload data

1. Create a SQL database called guestbook on your Cloud SQL instance:

```sql
CREATE DATABASE guestbook;
```

2. Insert the following sample data into the guestbook database:

```sql
USE guestbook;
CREATE TABLE entries (guestName VARCHAR(255), content VARCHAR(255),
    entryID INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(entryID));
INSERT INTO entries (guestName, content) values ("first guest", "I got here!");
INSERT INTO entries (guestName, content) values ("second guest", "Me too!");
```

3. Now retrieve the data:

```sql
SELECT * FROM entries;
```

You should see:

```sql
+--------------+-------------------+---------+
| guestName    | content           | entryID |
+--------------+-------------------+---------+
| first guest  | I got here!       |       1 |
| second guest | Me too!           |       2 |
+--------------+-------------------+---------+
2 rows in set (0.00 sec)
mysql>
```

## Spanner

Cloud Spanner is a fully managed, **horizontally scalable**, globally distributed, and **strongly consistent** relational database service. It is designed to handle large amounts of data and high transaction volumes while providing high availability and low latency.

Features:
* Scales horizontally: Horizontal scaling, also known as scale-out, involves adding more machines or nodes to a system to handle increased load or data. This allows for distributing the workload across multiple resources, leading to improved performance and resilience.
* Strongly consistent
* Speaks SQL

### Aplications that require

* SQL relational database management system with joins and secondary indexes.
* Built-in high availability.
* Strog global consistency.
* High numbers of input/output operations per second (IOPS). Tens of thousands of IOPS per node.

### How it works

Data is automatically and instantly copied across regions, which is called synchronous replication. As a result, queries always return consistent and ordered answers regardless of the region.

Google uses replication within and across regions to achieve availability, so if one region goes offline, the user’s data can still be served from another region.

## No SQL Databases

Google offers two managed NoSQL database options:

* **Firestore**: A fully managed, serverless NoSQL document store that supports ACID transactions.
* **Bigtable**: A petabyte-scale, sparse wide-column NoSQL database that offers extremely low write latency.

### Key Features of Firestore

**Flexible and Scalable**

Firestore is a NoSQL cloud database that supports mobile, web, and server development. It is horizontally scalable, allowing for seamless handling of growing data and traffic.

**Document-Centric Storage**

Data is stored in a document structure and organized into **collections**. Documents can include complex, nested objects and subcollections for hierarchical organization.

**Powerful Query Capabilities**

Firestore allows retrieval of specific documents or all documents matching query parameters with support for multiple, chained filters and combined filtering and sorting options. Automatic indexing ensures that query performance is determined by the size of the result set rather than the overall dataset.

**Data Synchronization**

Firestore keeps data updated across all connected devices in real-time. It supports offline mode, allowing applications to write, read, listen, and query data even when the device is offline. Local changes are synchronized back to the database when the device reconnects.

**Caching for Efficiency**

   - Actively used data is cached for quick access.
   - Designed for efficient, one-time fetch queries to minimize overhead.

**Built on Google Cloud's Robust Infrastructure**
   - **Automatic Multi-Region Data Replication**: Ensures availability and durability.
   - **Strong Consistency Guarantees**: Always provides the latest data across all devices.
   - **Atomic Batch Operations**: Supports batch writes that are executed as a single unit of work.
   - **Real Transactions**: Enables true ACID transactions for reliable and consistent operations.

### Advantages of Firestore

- **Real-Time Updates**: Ideal for collaborative, live applications.
- **Offline Support**: Perfect for apps with intermittent connectivity.
- **Query Optimization**: Fast and scalable querying with built-in indexing.
- **Serverless Architecture**: No need to manage servers—Firestore takes care of scaling and availability.

Firestore empowers developers to focus on building rich, dynamic applications without worrying about the complexity of database management.

## Bigtable

It can handle massive amounts of data and is designed for low-latency read and write operations. It is ideal for applications that require **high throughput** and **low latency**, such as **real-time analytics**, time-series data, and **large-scale machine learning**.

### Why to choose Bigtable

- Working with more than **1 TB** of **semi-structured** or **structured data**.
- **Fast data** with **high throughput** or **rapidly changing data**.
- Working with **NoSQL data**, where strong **relational semantics** are not required.
- Data is a **time-series** or has natural **semantic ordering**.
- Handling **big data** with **asynchronous batch** or **synchronous real-time processing**.
- Running **machine learning algorithms** on **large datasets**.

Bigtable can interact with other **Google Cloud services** and **third-party clients**. Using **APIs**, data can be read from and written to Bigtable through a **data service layer** like **Managed VMs**, the **HBase REST Server**, or a **Java Server** using the **HBase client**.

Typically this is used to serve data to **applications**, **dashboards**, and **data services**. Data can also be streamed in through various popular **stream processing frameworks** like **Dataflow Streaming**, **Spark Streaming**, and **Storm**. And if streaming isn’t an option, data can also be read from and written to Bigtable through **batch processes** like **Hadoop MapReduce**, **Dataflow**, or **Spark**.

