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


## Apigee API Management

Apigee API Management is a Google Cloud platform designed for developing and managing API proxies. It focuses on solving business challenges such as:

- **Rate Limiting**
- **Quotas**
- **Analytics**

### Key Features:

- Ideal for companies offering software services to other businesses.
- Supports backend services outside Google Cloud.
- Useful for modernizing legacy applications.

### Modernizing Legacy Applications:

Instead of replacing an entire legacy application at once, Apigee enables a gradual migration by:

1. Isolating individual services from the legacy application.
2. Rebuilding these services as **microservices**.
3. Iteratively implementing microservices until the legacy application can be fully retired.

#### Challenges to data ingestion

1. Data can be streamed from many different sources, such as IoT devices, mobile applications, and web applications.
2. It can be hard to distribute event messages to the right subscribers.
3. Data can arrive quickly and in large volumes.
3. Ensuring services are reliable and secure, and perform as expected.

## Pub/Sub

Pub/Sub, short for Publisher/Subscriber, is a distributed messaging service that handles messages from various sources like gaming events, IoT devices, and application streams. It ensures at-least-once message delivery to subscribers without requiring provisioning. Pub/Sub offers open APIs, global availability, and end-to-end encryption.

### Pub/Sub Architecture:
1. **Data Ingestion**: Data streams from devices worldwide are ingested into Pub/Sub, acting as the initial entry point.
2. **Message Broadcasting**: Pub/Sub stores and broadcasts data to subscribers of specific topics.
3. **Data Processing**: Subscribers like Dataflow use elastic pipelines to transform data and send results to analytics platforms like BigQuery.
4. **Visualization & AI**: Tools such as Looker Studio or Vertex AI can visualize and analyze the processed data for insights or predictions.

### Key Concept: Topics
A topic is a named resource where publishers send messages. Subscribers listen for messages on these topics. Publishers and subscribers are decoupled, meaning they can operate independently without impacting each other.

### Example Use Case:
An HR system publishes events like new employee notifications to a Pub/Sub topic. Multiple systems, such as directory services or badge activation, subscribe to this topic and independently process the updates. Pub/Sub handles multiple publishers and subscribers efficiently, ensuring reliable message distribution.

Pub/Sub is ideal for loosely coupled architectures with diverse data sources and destinations. It supports various input/output systems and allows chaining topics for advanced workflows.

API management system supports applications running in App Engine, Google Kubernetes Engine, and Compute Engine is

## Security in Google Cloud

Google Cloud has different fiver layers of protection:

### Hardware infrastructure

- **Hardware design and provenance**: Both the server boards and the networking equipment in Google data centers are custom designed by Google. Google also designs custom chips, including a hardware security chip that's currently being deployed on both servers and peripherals.

- **Secure boot stack**: Google server machines use various technologies to ensure that they are booting the correct software stack, such as cryptographic signatures over the BIOS, bootloader, kernel, and base operating system image.

- **Premises security**: Google designs and builds its own data centers, which incorporate multiple layers of physical security protections.

### Service deployment

- **Encryption of inter-service communication**: Google’s infrastructure provides cryptographic privacy and integrity for remote procedure call (“RPC”) data on the network. Google’s services communicate with each other using RPC calls. The infrastructure automatically encrypts all infrastructure RPC traffic that goes between data centers. Google has started to deploy hardware cryptographic accelerators that will allow it to extend this default encryption to all infrastructure RPC traffic inside Google data centers.

- **User Identity**: Google’s central identity service, typically manifested as the Google login page, goes beyond asking for a simple username and password. The service intelligently challenges users for additional information based on risk factors, such as whether they have logged in from the same device or a similar location in the past. Users also have the option of employing secondary factors when signing in, including devices based on the Universal 2nd Factor (U2F) open standard.

### Storage Services

- **Encryption at Rest**: Most applications at Google access physical storage (in other words, “file storage”) indirectly by using storage services. Encryption, using centrally managed keys, is applied at the layer of these storage services. Additionally, Google enables hardware encryption support in hard drives and SSDs.

### Internet communication

* **Google Front End (GFE)**: Google services that want to make themselves available on the internet register themselves with an infrastructure service called the Google Front End, which ensures that all TLS connections are ended using a public-private key pair and an X.509 certificate from a Certified Authority (CA), and follows best practices such as supporting perfect forward secrecy. 

The GFE also applies protections against Denial of Service attacks.

* **Denial of Service (DoS) protection**: The sheer scale of its infrastructure enables Google to simply absorb many DoS attacks. Google also has multi-tier, multi-layer DoS protections that further reduce the risk of any DoS impact on a service running behind a GFE.

### Operational security

- **Intrusion Detection**: Rules and machine intelligence give Google’s operational security teams warnings of possible incidents. Google conducts Red Team exercises to measure and improve the effectiveness of its detection and response mechanisms.

- **Reducing Insider Risk**: Google aggressively limits and actively monitors the activities of employees who have been granted administrative access to the infrastructure.

- **Employee U2F Use**: To guard against phishing attacks against Google employees, employee accounts require use of U2F-compatible security keys.

- **Software Development Practices**: Google employs central source control and requires two-party review of new code.

## Shared security model

Security responsibilities are shared between the customer and Google Cloud. The upper layers of the stack, such as applications and data, are the customer's responsibility, while Google Cloud manages the lower layers, including hardware, software, networking, and facilities, depending on the type of service (IaaS, Paass, etc).

## Encrypt data options

### Encryption Methods in Google Cloud

Google Cloud provides several options to protect data through encryption:

1. **Default Encryption by Google Cloud**
   - Data in transit is encrypted using Transport Layer Security (TLS).
   - Data at rest is automatically encrypted with AES 256-bit keys.

2. **Customer-Managed Encryption Keys (CMEK)**
   - Customers manage their own encryption keys, protected by Google Cloud.
   - Uses the Cloud Key Management Service (Cloud KMS) to generate and manage keys easily.
   - Supports symmetric and asymmetric keys, allowing encryption, decryption, signing, and verification.
   - Supports manual and automatic key rotation.

3. **Customer-Supplied Encryption Keys (CSEK)**
   - Users generate and manage their own AES 256-bit keys.
   - Keys only exist in memory during the encryption process and are discarded afterward.
   - Offers greater control but requires more complex management.

4. **Client-Side Encryption**
   - Data is encrypted locally before being sent to Google Cloud.
   - Neither the unencrypted data nor the decryption keys leave the local device.

### Persistent Disk Encryption

- Persistent disks are encrypted by default even without using CSEK or CMEK.
- When a persistent disk is deleted, its keys are discarded, making the data irrecoverable.
- Users can create redundant persistent disks for greater control over encryption.

## IAM Overview

With IAM, administrators can create policies defining who can perform specific actions on particular resources. 

### Key Components:
1. **Who**: The policy’s subject can be a Google Account, Google Group, Service Account, or Cloud Identity domain.
2. **What They Can Do**: Defined by roles, which are collections of permissions grouped together for easier management.

When assigned to a user, group, or service account, a role applies to the specified resource and all its sub-resources in the hierarchy. **Deny rules** can be created to explicitly block certain permissions, taking precedence over allow policies.

### Cloud Identity Integration:
Cloud Identity allows organizations to manage users and groups centrally through the Google Admin console. Admins can:
- Use existing credentials from systems like Active Directory or LDAP.
- Disable accounts or remove users' access when they leave the organization.

Cloud Identity is available in both free and premium editions.

### IAM Roles:
IAM roles come in three types:
1. **Basic Roles**: Broad in scope, such as:
   - **Owner**: Full access, including role management and billing.
   - **Editor**: Modify resources.
   - **Viewer**: Read-only access.
   - **Billing Administrator**: Manage billing without altering resources.

   Basic roles are often too broad for sensitive projects.

2. **Predefined Roles**: Specific to Google Cloud services, allowing fine-grained permissions for particular actions on services like Compute Engine. For example:
   - The “instanceAdmin” role for managing virtual machines.

3. **Custom Roles**: Allow organizations to define roles with minimal privileges tailored to their needs, such as granting only start/stop permissions for VMs. However:
   - Permissions within custom roles must be actively managed.
   - Custom roles are limited to project or organization-level assignments.

### Service Accounts:
Service accounts enable secure interaction between applications and Google Cloud resources. Key details:
- Identified by email addresses and authenticate using cryptographic keys.
- Can be assigned specific roles, such as Compute Engine’s Instance Admin role, to grant applications restricted access.

## Identity-Aware Proxy (IAP)
Identity-Aware Proxy (IAP) is a Google Cloud resource used for securing access to HTTPS-based applications without relying on VPNs. It provides a centralized authorization layer over TLS, enabling application-level access control instead of traditional network-level firewalls.

### Key Features of IAP:
- **Granular Access Control**: Only users and groups with the appropriate IAM roles can access applications and resources protected by IAP.
- **Protection Layer**: Acts as a proxy, shielding internal services from the outside world.
- **Authentication & Authorization**: Performs checks whenever a user tries to access an IAP-secured resource.
- **TLS Encryption**: Secures external requests through Transport Layer Security.

### Limitations:
- **Internal VM Activities**: IAP does not safeguard against actions inside the VM, such as SSH access.
- **Project-Level Activities**: Does not protect interactions within a project, e.g., VM-to-VM communications over the local network.

## Best Practices for Authorization

### Resource Hierarchy
- Group resources into projects that share the same trust boundary. Check and understand the policies granted on each resource, including inheritance.
- Apply the principle of least privilege when granting roles.
- Audit policies using Cloud Audit Logs and review memberships of groups in policies.

### Using Groups for Role Assignment
- Assign roles to groups instead of individuals to simplify updates.
- Regularly audit group memberships and control ownership of Google groups used in IAM policies.
- Use multiple groups for granular control. For example:
    - A network admin group with subsets for `read_write` and `read_only` roles.

### Service Accounts
- Be cautious when granting the Service Account Users role, as it provides access to all resources accessible by the service account.
- Use clear and consistent naming conventions for service accounts and their display names.
- Establish key rotation policies and methods for service account keys.
