# Google Cloud CLI Commands

This document serves as a comprehensive guide for Google Cloud CLI commands, presented with general command structures followed by customizable options. Simply copy and paste the base command, then add the required options as needed.

---

## General Configuration

### Set Compute Region

```bash
gcloud config set compute/region REGION
```

Options:

- `REGION`: Replace with your desired compute region (e.g., `us-east1`).

### Set Compute Zone

```bash
gcloud config set compute/zone ZONE
```

Options:

- `ZONE`: Replace with your desired compute zone (e.g., `us-east1-b`).

---

## Compute Engine

### Create a VM Instance

```bash
gcloud compute instances create INSTANCE_NAME
```

Options:

- **Machine Type**:
  **Description**: Specifies the machine type.
  **Examples**:

  ```bash
  --machine-type=e2-medium
  --machine-type=n1-standard-1
  --machine-type=e2-micro
  --machine-type=e2-small
  --machine-type=n1-standard-2
  --machine-type=e2-highcpu-4
  --machine-type=e2-highmem-8
  ```

- **Zone**:
  **Description**: Specifies the zone.
  **Examples**:

  ```bash
  --zone=$ZONE
  --zone=us-east1-b
  --zone=us-central1-a
  --zone=europe-west1-b
  ```

- **Project ID**:
  **Description**: Specifies the project ID.
  **Examples**:

  ```bash
  --project=my-project-id
  ```

- **Tags**:  
  **Description**: Adds network tags for firewall rules.  
  **Examples**:

  ```bash
  --tags=web-server
  --tags=secure-backend
  ```

- **Labels**:
  **Description**: Specifies labels for the instance.
  **Examples key-values pairs**:

  ```bash
  --labels=KEY=VALUE
  --labels=env=production
  --labels=team=devops
  ```

- **Image Family**:
  **Description**: Specifies the image to use for the instance.
  **Examples**:

  ```bash
  --image-family=debian-11
  --image-family=ubuntu-2004-focal-v20230420
  ```

- **Image Project**:
  **Description**: Specifies the project containing the image.
  **Examples**:

  ```bash
  --image-project=debian-cloud
  --image-project=ubuntu-os-cloud
  ```

- **Boot Disk Size**:
  **Description**: Specifies the boot disk size.
  **Examples**:

  ```bash
  --boot-disk-size=10GB
  --boot-disk-size=50GB
  --boot-disk-size=100GB
  ```

- **Boot Disk Type**:
  **Description**: Specifies the boot disk type.
  **Examples**:

  ```bash
  --boot-disk-type=pd-standard
  --boot-disk-type=pd-ssd
  --boot-disk-type=pd-balanced
  ```

- **Boot Disk Device Name**:
  **Description**: Specifies the device name for the boot disk.
  **Examples**:

  ```bash
  --boot-disk-device-name=boot-disk-1
  --boot-disk-device-name=primary-disk
  ```

- **Metadata**:
  **Description**: Adds metadata to the instance.
  **Examples**:

  ```bash
  --metadata=startup-script-url=gs://bucket/script.sh
  --metadata=enable-oslogin=TRUE
  ```

- **Metadata from File**:
  **Description**: Adds metadata from a file.
  **Examples**:

  ```bash
  --metadata-from-file=startup-script=path/to/startup.sh
  ```

- **Network Interface**:
  **Description**: Defines the network interfaces.
  **Examples**:

  ```bash
  --network-interface=nic0
  --network-interface=nic1
  ```

- **Maintenance Policy**:
  **Description**: Defines the maintenance policy.
  **Examples**:

  ```bash
  --maintenance-policy=MIGRATE
  --maintenance-policy=TERMINATE
  --maintenance-policy=RESTART
  ```

- **Preemptible**:  
  **Description**: Creates a preemptible instance.  
  **Examples**:

  ```bash
  --preemptible
  ```

- **Reservation Affinity**:  
  **Description**: Specifies reservation affinity.  
  **Examples**:  

  ```bash
  --reservation-affinity=any
  --reservation-affinity=specific
  --reservation-affinity=none
  ```

### SSH into a VM Instance

```bash
gcloud compute ssh INSTANCE_NAME
```

Options:

- **Zone**:
  **Description**: Specify the zone of the instance.
  **Examples**:

  ```bash
  --zone=us-east1-b
  --zone=us-central1-a
  ```

### List VM Instances

```bash
gcloud compute instances list
```

---

## VPC Networks

### Create a VPC Network

```bash
gcloud compute networks create NETWORK_NAME
```

Options:

- `--subnet-mode SUBNET_MODE`: Specify `custom` or `auto`.

### Create a Subnet

```bash
gcloud compute networks subnets create SUBNET_NAME
```

Options:

- `--network NETWORK_NAME`: Specify the VPC network.
- `--region REGION`: Specify the region for the subnet.
- `--range CIDR_RANGE`: Specify the CIDR range (e.g., `192.168.1.0/24`).

### List VPC Networks

```bash
gcloud compute networks list
```

---

## Firewall Rules

### Create a Firewall Rule

```bash
gcloud compute firewall-rules create RULE_NAME
```

Options:

- **Direction**:
  **Description**: Specify the rule direction.
  **Examples**:

  ```bash
  --direction=INGRESS
  --direction=EGRESS
  ```

- **Priority**:
  **Description**: Set the priority.
  **Examples**:

  ```bash
  --priority=1000
  --priority=500
  ```

- `--network NETWORK_NAME`: Specify the VPC network.
- **Action**:
  **Description**: Specify the action.
  **Examples**:

  ```bash
  --action=ALLOW
  --action=DENY
  ```

- **Protocols and Ports**:
  **Description**: Define the protocols and ports for the firewall rule.
  **Examples**:

  ```bash
  --rules=tcp:22,tcp:80
  ```

  **Alternative**:

  ```bash
  --rules=udp:53
  ```

- **Source Ranges**:
  **Description**: Define the source ranges.
  **Examples**:

  ```bash
  --source-ranges=0.0.0.0/0
  ```

### List All Firewall Rules

```bash
gcloud compute firewall-rules list
```

---

## Cloud Storage

### Create a Bucket

```bash
gcloud storage buckets create gs://BUCKET_NAME
```

Options:

- `--location LOCATION`: Specify the bucket location (e.g., `US`).

- `--action=ACTION`: Specify the action for the command.
  **Examples**:

  ```bash
  --action=ALLOW
  --action=DENY
  ```

- **Storage Class**:
  **Description**: Define the storage class.
  **Examples**:

  ```bash
  --storage-class=STANDARD
  --storage-class=NEARLINE
  ```

### Upload an Object

```bash
gcloud storage cp LOCAL_FILE_PATH gs://BUCKET_NAME
```

### Download an Object

```bash
gcloud storage cp gs://BUCKET_NAME/OBJECT_NAME LOCAL_FILE_PATH
```

### List Bucket Contents

```bash
gcloud storage ls gs://BUCKET_NAME
```

### Delete a Bucket

```bash
gcloud storage buckets delete gs://BUCKET_NAME
```

---

## Cloud SQL

### Connect to an Instance

```bash
gcloud sql connect INSTANCE_NAME
```

Options:

- **Database User**:
  **Description**: Specify the database user.
  **Examples**:

  ```bash
  --user=admin
  ```

---

## Dataproc

### Create a Cluster

```bash
gcloud dataproc clusters create CLUSTER_NAME
```

Options:

- **Region**:
  **Description**: Specify the region.
  **Examples**:

  ```bash
  --region=us-east1
  --region=europe-west1
  ```

- **Number of Workers**:
  **Description**: Set the number of workers.
  **Examples**:

  ```bash
  --num-workers=3
  --num-workers=5
  ```

- **Worker Machine Type**:
  **Description**: Define the machine type for workers.
  **Examples**:

  ```bash
  --worker-machine-type=n1-standard-2
  --worker-machine-type=n1-highmem-4
  ```

### Submit a Spark Job

```bash
gcloud dataproc jobs submit spark
```

Options:

- `--cluster CLUSTER_NAME`: Specify the cluster.
- `--class MAIN_CLASS`: Define the main class for the job.
- `--jars JAR_FILES`: Provide paths to JAR files.

### Delete a Cluster

```bash
gcloud dataproc clusters delete CLUSTER_NAME
```

---

## BigQuery

### Create a Dataset

```bash
bq mk DATASET_NAME
```

### Create a Table

```bash
bq mk --table TABLE_NAME SCHEMA
```

Options:

- `SCHEMA`: Define the table schema (e.g., `name:STRING,age:INTEGER`).

### Query Data

```bash
bq query --use_legacy_sql=false 'SQL_QUERY'
```

---

## Kubernetes Engine

### Create a GKE Cluster

```bash
gcloud container clusters create CLUSTER_NAME
```

Options:

- `--num-nodes NUM_NODES`: Set the number of nodes.
- `--zone ZONE`: Specify the zone.

### Get Cluster Credentials

```bash
gcloud container clusters get-credentials CLUSTER_NAME
```

Options:

- `--zone ZONE`: Specify the zone.

### Delete a Cluster

```bash
gcloud container clusters delete CLUSTER_NAME
```

---

## Pub/Sub

### Create a Topic

```bash
gcloud pubsub topics create TOPIC_NAME
```

### Create a Subscription

```bash
gcloud pubsub subscriptions create SUBSCRIPTION_NAME
```

Options:

- **Topic**:
  **Description**: Specify the topic.
  **Examples**:

  ```bash
  --topic=example-topic
  ```

### Publish a Message

```bash
gcloud pubsub topics publish TOPIC_NAME --message "MESSAGE"
```

### Pull Messages

```bash
gcloud pubsub subscriptions pull SUBSCRIPTION_NAME
```

Options:

- **Auto Acknowledge**:
  **Description**: Automatically acknowledge messages.
  **Examples**:

  ```bash
  --auto-ack
  ```

---

## Monitoring and Logging

### View Logs

```bash
gcloud logging read 'LOG_FILTER'
```

Options:

- **Limit Records**:
  **Description**: Limit the number of records.
  **Examples**:

  ```bash
  --limit=50
  ```

---

This document is a living guide and will be updated as new Google Cloud CLI commands and options become available.
