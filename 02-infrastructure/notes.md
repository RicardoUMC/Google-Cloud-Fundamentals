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

### **Make bucket command:**

```bash
gcloud storage buckets create gs://<YOUR-BUCKET-NAME>
```

### **Upload an object into your bucket:**

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

### **Download an object from your bucket:**

```bash
gcloud storage cp -r gs://YOUR-BUCKET-NAME/ada.jpg .

```

### **Copy an object to a folder in the bucket:**

```bash
gcloud storage cp gs://YOUR-BUCKET-NAME/ada.jpg gs://YOUR-BUCKET-NAME/image-folder/

```

### **List contents of a bucket or folder:**

```bash
gcloud storage ls gs://YOUR-BUCKET-NAME

```

### **List details for an object:**

```bash
gcloud storage ls -l gs://YOUR-BUCKET-NAME/ada.jpg

```

### **Make your object publicly accessible:**

```bash
gsutil acl ch -u AllUsers:R gs://YOUR-BUCKET-NAME/ada.jpg

```

### **Remove public access:**

```bash
gsutil acl ch -d AllUsers gs://YOUR-BUCKET-NAME/ada.jpg

```

### **Delete objects:**

```bash
gcloud storage rm gs://YOUR-BUCKET-NAME/ada.jpg
```
