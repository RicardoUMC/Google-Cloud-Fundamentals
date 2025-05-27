# Google Cloud Setup and App Engine Environments

## Configure Project Region

```bash
gcloud config set compute/region us-east4
````

## Create Environment Variables

Set a variable for the region:

```bash
export REGION=us-east4
```

Set a variable for the zone:

```bash
export ZONE=us-east4-b
```

## Create a VM Instance with gcloud

```bash
gcloud compute instances create gcelab2 --machine-type e2-medium --zone=$ZONE
```

## View Default Configuration Help

```bash
gcloud compute instances create --help
```

## Connect via SSH Using gcloud

```bash
gcloud compute ssh gcelab2 --zone=us-east4-b
```

---

## App Engine Environments

There are two types of App Engine environments: **Standard** and **Flexible**.

### Standard Environment

Based on container instances running on Google's infrastructure. Features include:

* Persistent storage with queries, sorting, and transactions.
* Automatic scaling and load balancing.
* Asynchronous task queues for executing work outside the scope of a request.
* Scheduled tasks to trigger events at specific times or intervals.
* Integration with other Google Cloud services and APIs.

**Requirements:**

1. You must use specific versions of Java, Python, PHP, Go, Node.js, or Ruby.
2. Your application must comply with sandbox constraints, which depend on the runtime environment.

**Workflow:**

1. The web application is first developed and tested locally.
2. The SDK is then used to deploy the application to App Engine.
3. App Engine automatically scales and serves the application.

### Flexible Environment

The Flexible Environment allows you to define the type of container your web application runs in. This option enables your app to run inside Docker containers hosted on Google Cloud’s Compute Engine VMs.
