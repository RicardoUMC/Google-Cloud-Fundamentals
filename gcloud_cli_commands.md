# Google Cloud CLI Commands

This document serves as a comprehensive guide for Google Cloud CLI commands, presented with general command structures followed by customizable options. Simply copy and paste the base command, then add the required options as needed.

---

## General Configuration

### Set Compute Region

```bash
gcloud config set compute/region REGION
```

Options:

- `REGION`: Replace with your desired compute region.

  **Examples:**

  ```bash
  us-east1
  us-central1
  europe-west1
  ```

### Set Compute Zone

```bash
gcloud config set compute/zone ZONE
```

Options:

- `ZONE`: Replace with your desired compute zone.

  **Examples:**

  ```bash
  us-east1-b
  us-central1-a
  europe-west1-b
  ```

---

## Compute Engine

### Create a VM Instance

```bash
gcloud compute instances create INSTANCE_NAME
```

Options:

- **Machine Type**: Specifies the machine type.

  ```bash
  --machine-type=e2-medium
  --machine-type=n1-standard-1
  --machine-type=e2-micro
  --machine-type=e2-small
  --machine-type=n1-standard-2
  --machine-type=e2-highcpu-4
  --machine-type=e2-highmem-8
  ```

- **Zone**: Specifies the zone.

  ```bash
  --zone=$ZONE
  --zone=us-east1-b
  --zone=us-central1-a
  --zone=europe-west1-b
  ```

- **Project ID**: Specifies the project ID.

  ```bash
  --project=my-project-id
  ```

- **Tags**: Adds network tags for firewall rules.

  ```bash
  --tags=web-server
  --tags=secure-backend
  ```

- **Labels**: Specifies labels for the instance.

  ```bash
  --labels=KEY=VALUE
  --labels=env=production
  --labels=team=devops
  ```

- **Image Family**: Specifies the image to use for the instance.

  ```bash
  --image-family=debian-11
  --image-family=ubuntu-2004-focal-v20230420
  ```

- **Image Project**: Specifies the project containing the image.

  ```bash
  --image-project=debian-cloud
  --image-project=ubuntu-os-cloud
  --image-project=centos-cloud
  --image-project=windows-cloud
  --image-project=coreos-cloud
  ```

- **Boot Disk Size**: Specifies the boot disk size.

  ```bash
  --boot-disk-size=10GB
  --boot-disk-size=50GB
  --boot-disk-size=100GB
  ```

- **Boot Disk Type**: Specifies the boot disk type.

  ```bash
  --boot-disk-type=pd-standard
  --boot-disk-type=pd-ssd
  --boot-disk-type=pd-balanced
  ```

- **Boot Disk Device Name**: Specifies the device name for the boot disk.

  ```bash
  --boot-disk-device-name=boot-disk-1
  --boot-disk-device-name=primary-disk
  --boot-disk-device-name=default
  --boot-disk-device-name=my-custom-disk
  ```

- **Metadata**: Adds metadata to the instance.

  ```bash
  --metadata=startup-script-url=gs://bucket/script.sh
  --metadata=enable-oslogin=TRUE
  ```

- **Metadata from File**: Adds metadata from a file.

  ```bash
  --metadata-from-file=startup-script=path/to/startup.sh
  ```

- **Network Interface**: Defines the network interfaces.

  ```bash
  --network-interface=nic0
  --network-interface=nic1
  ```

- **Maintenance Policy**: Defines the maintenance policy.

  ```bash
  --maintenance-policy=MIGRATE
  --maintenance-policy=TERMINATE
  --maintenance-policy=RESTART
  ```

- **Preemptible**: Creates a preemptible instance.

  ```bash
  --preemptible
  ```

- **Reservation Affinity**: Specifies reservation affinity.

  ```bash
  --reservation-affinity=any
  --reservation-affinity=specific:RESOURCE_NAME
  --reservation-affinity=none
  ```

### SSH into a VM Instance

```bash
gcloud compute ssh INSTANCE_NAME
```

Options:

- **Zone**: Specify the zone of the instance.

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

- **Subnet Mode**: Specify the subnet mode.

  ```bash
  --subnet-mode=custom
  --subnet-mode=auto
  ```

### Create a Subnet

```bash
gcloud compute networks subnets create SUBNET_NAME
```

Options:

- **Network**: Specifies the VPC network.

  ```bash
  --network=NETWORK_NAME
  --network=default
  --network=custom-network
  ```

- **Region**: Specifies the region for the subnet.

  ```bash
  --region=REGION
  ```

- **CIDR Range**: Specifies the CIDR range for the subnet.

  ```bash
  --range=192.168.1.0/24
  ```

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

- **Direction**: Specify the rule direction.

  ```bash
  --direction=INGRESS
  --direction=EGRESS
  ```

- **Priority**: Set the priority.

  ```bash
  --priority=1000
  --priority=500
  ```

- **Network**: Specify the VPC network.

  ```bash
  --network=NETWORK_NAME
  ```

- **Action**: Specify the action.

  ```bash
  --action=ALLOW
  --action=DENY
  --action=LOG
  ```

- **Protocols and Ports**: Define the protocols and ports for the firewall rule.

  ```bash
  --rules=tcp:22,tcp:80
  --rules=tcp:443
  --rules=udp:123
  --rules=icmp
  --rules=tcp:8080,udp:53
  ```

- **Source Ranges**: Define the source ranges.

  ```bash
  --source-ranges=0.0.0.0/0
  --source-ranges=192.168.1.0/24
  --source-ranges=10.0.0.0/8
  --source-ranges=172.16.0.0/12
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

- **Location**: Specify the bucket location.

  ```bash
  --location=US
  --location=EU
  ```

- **Action**: Specify the action for the command.

  ```bash
  --action=ALLOW
  --action=DENY
  ```

- **Storage Class**: Define the storage class.

  ```bash
  --storage-class=STANDARD
  --storage-class=NEARLINE
  --storage-class=COLDLINE
  --storage-class=ARCHIVE
  ```
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

- **Database User**: Specify the database user.

  ```bash
  --user=admin
  --user=readonly
  --user=application-user
  ```

---

## Dataproc

### Create a Cluster

```bash
gcloud dataproc clusters create CLUSTER_NAME
```

Options:

- **Region**: Specify the region.

  ```bash
  --region=us-east1
  --region=europe-west1
  ```

- **Number of Workers**: Set the number of workers.

  ```bash
  --num-workers=3
  --num-workers=5
  ```

- **Worker Machine Type**: Define the machine type for workers.

  ```bash
  --worker-machine-type=n1-standard-2
  --worker-machine-type=n1-standard-4
  --worker-machine-type=n1-standard-8
  --worker-machine-type=n1-standard-16
  --worker-machine-type=n1-standard-32
  --worker-machine-type=n1-standard-64
  --worker-machine-type=n1-highmem-2
  --worker-machine-type=n1-highmem-4
  --worker-machine-type=n1-highmem-8
  --worker-machine-type=n1-highmem-16
  --worker-machine-type=n1-highmem-32
  --worker-machine-type=n1-highmem-64
  --worker-machine-type=n1-highcpu-2
  --worker-machine-type=n1-highcpu-4
  --worker-machine-type=n1-highcpu-8
  --worker-machine-type=n1-highcpu-16
  --worker-machine-type=n1-highcpu-32
  --worker-machine-type=n1-highcpu-64
  --worker-machine-type=e2-standard-2
  --worker-machine-type=e2-standard-4
  --worker-machine-type=e2-standard-8
  --worker-machine-type=e2-standard-16
  --worker-machine-type=e2-highmem-2
  --worker-machine-type=e2-highmem-4
  --worker-machine-type=e2-highmem-8
  --worker-machine-type=e2-highmem-16
  --worker-machine-type=e2-highcpu-2
  --worker-machine-type=e2-highcpu-4
  --worker-machine-type=e2-highcpu-8
  --worker-machine-type=e2-highcpu-16
  --worker-machine-type=c2-standard-4
  --worker-machine-type=c2-standard-8
  --worker-machine-type=c2-standard-16
  --worker-machine-type=c2-standard-30
  --worker-machine-type=c2-standard-60
  ```

### Submit a Spark Job

```bash
gcloud dataproc jobs submit spark
```

Options:

- **Cluster**: Specify the cluster.

  ```bash
  --cluster=CLUSTER_NAME
  ```

- **Main Class**: Define the main class for the job.

  ```bash
  --class=MAIN_CLASS
  ```

- **JAR Files**: Provide paths to JAR files.

  ```bash
  --jars=JAR_FILES
  ```

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

- **Schema**: Define the table schema.

  ```bash
  name:STRING,age:INTEGER,email:STRING,joined_date:DATE,active:BOOLEAN
  ```

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

- **Number of Nodes**: Set the number of nodes.

  ```bash
  --num-nodes=NUM_NODES
  ```

- **Zone**: Specify the zone.

  ```bash
  --zone=ZONE
  ```

### Get Cluster Credentials

```bash
gcloud container clusters get-credentials CLUSTER_NAME
```

Options:

- **Zone**: Specify the zone.

  ```bash
  --zone=ZONE
  ```

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

- **Topic**: Specify the topic.

  ```bash
  --topic=example-topic
  --topic=my-topic
  --topic=project-topic
  --topic=cloud-functions-topic
  --topic=data-stream-topic
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

- **Auto Acknowledge**: Automatically acknowledge messages.

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

- **Limit Records**: Limit the number of records.

  ```bash
  --limit=50
  ```

---

This document is a living guide and will be updated as new Google Cloud CLI commands and options become available.
