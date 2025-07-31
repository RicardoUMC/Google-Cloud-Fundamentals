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

Th#e Flexible Environment allows you to define the type of container your web application runs in. This option enables your app to run inside Docker containers hosted on Google Cloud’s Compute Engine VMs (App Engine manages Compute Engine machines for you). Features include:

* Instances are health-checked, healed as necessary, and colocated with other module instances within the project.
* Critical, backward-compatible updates are automatically applied to the underlying operating system.
* VM instances are automatically located by geographical region according to the settings in your project.
* And VM instances are restarted on a weekly basis.

The flexible environment supports microservices, authorization, SQL and NoSQL databases, traffic splitting, logging, search, versioning, security scanning, Memcache, and content delivery networks.

**Advantages**

1. App Engine flexible environment allows users to also benefit from custom configurations and libraries while still keeping their main focus on what they do best – writing code.
2. As in the App Engine standard environment, supported runtimes include Python, Java, Go, Node.js, PHP, and Ruby.
3. Developers can also use different versions of these runtimes or provide their own custom runtime by supplying a custom Docker image or using a Dockerfile from the open source community.

### Comparison

| Feature                     | Standard Environment                                                                 | Flexible Environment                                                |
|----------------------------|---------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| **Instance startup**       | Seconds                                                                              | Minutes                                                             |
| **SSH access**             | No                                                                                   | Yes (although not by default)                                      |
| **Write to local disk**    | No (some runtimes have read and write access to the `/tmp` directory)                | Yes, ephemeral (disk initialized on each VM startup)               |
| **Support for 3rd-party binaries** | For certain languages                                                        | Yes                                                                 |
| **Network access**         | Via App Engine services                                                              | Yes                                                                 |
| **Pricing model**          | After free tier usage, pay per instance class, with automatic shutdown               | Pay for resource allocation per hour; no automatic shutdown         |

## Cloud Run Functions

1. In Cloud Shell, run the following command to set the default region:

```bash
gcloud config set run/region REGION
```

2. Create a directory for the function code:

```bash
mkdir gcf_hello_world && cd $_
```

3. Create and open `index.js` to edit:

```bash
vim index.js
```

4. Copy the following into the `index.js` file:

```js
const functions = require('@google-cloud/functions-framework');

// Register a CloudEvent callback with the Functions Framework that will
// be executed when the Pub/Sub trigger topic receives a message.
functions.cloudEvent('helloPubSub', cloudEvent => {
    // The Pub/Sub message is passed as the CloudEvent's data payload.
    const base64name = cloudEvent.data.message.data;

      const name = base64name
        ? Buffer.from(base64name, 'base64').toString()
        : 'World';

     console.log(`Hello, ${name}!`);
});
```

5. Save and exit (Esc > ZZ)

6. Create and open `package.json` to edit:

7. Copy the following into the `package.json` file:

```json
{
    "name": "gcf_hello_world",
    "version": "1.0.0",
    "main": "index.js",
    "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
    },
    "dependencies": {
    "@google-cloud/functions-framework": "^3.0.0"
    }
}
```

8. Install the package dependencies

```bash
npm install
```

### Deploy your function

1. Deploy the **nodejs-pubsub-function** function to a pub/sub topic named **cf-demo**

```bash
gcloud functions deploy nodejs-pubsub-function \
    --gen2 \
    --runtime=nodejs20 \
    --region=REGION \
    --source=. \
    --entry-point=helloPubSub \
    --trigger-topic cf-demo \
    --stage-bucket PROJECT_ID-bucket \
    --service-account cloudfunctionsa@PROJECT_ID.iam.gserviceaccount.com \
    --allow-unauthenticated
```

> [!NOTE]
> If you get a service account serviceAccountTokenCreator notification select "n".

2. Verify the status of the function:

```bash
gcloud functions describe nodejs-pubsub-function \
    --region=REGION
```

### Test the function

Invoke the PubSub with some data.

```bash
gcloud pubsub topics publish cf-demo --message="Cloud Function Gen2"
```

### View logs

Check the logs to see your messages in the log history:

```bash
gcloud functions logs read nodejs-pubsub-function \
    --region=REGION
```

> [!NOTE]
> The logs can take around 10 mins to appear. Also, the alternative way to view the logs is, go to **Logging > Logs Explorer**.

## Google Kubernetes Engine

**Container**

A container is intended to provide workload-independent scalability in **PaaS** and an OS and hardware abstraction layer in **IaaS**.

A configurable system allows you to install your favorite web server, database, or middleware runtime environment, configure underlying system resources such as disk space, disk I/O, or networking tools, and compile to your liking.

Invisible box that surrounds code and its dependencies and has limited access to its own file system partition and hardware. Only requires a few system calls to create and starts as quickly as a process. All that is needed on each host is a container-capable OS kernel and a container runtime environment. Escala como las PaaS, pero brinda casi la misma flexibilidad que las IaaS.

### Kubernetes

Container orchestration tool for simplifying the management of containerized environments. Can be install Kubernetes on a group of your managed servers or run it as a hosted service on Google Cloud in a cluster of Compute Engine managed instances called Google Kubernetes Engine.

Allows you to:

* Install the system on local servers in the cloud
* Manage container networking and data storage
* Deploy rollouts and rollbacks
* Monitor and manage container and host health

### Set a default compute zone

Your compute zone is an approximate regional location in which your clusters and their resources live. For example, us-central1-a is a zone in the us-central1 region.

Set the default compute region:

```bash
gcloud config set compute/region "REGION"
```

Set the default compute zone:

```bash
gcloud config set compute/zone "ZONE"
```

### Create a GKE cluster

A cluster consists of at least one cluster master machine and multiple worker machines called nodes. Nodes are Compute Engine virtual machine (VM) instances that run the Kubernetes processes necessary to make them part of the cluster.

```bash
gcloud container clusters create --machine-type=e2-medium --zone=ZONE lab-cluster
```

### Get authentication credentials for the cluster

```bash
gcloud container clusters get-credentials lab-cluster
```

### Deploy an application to the cluster

1. To **create a new Deployment** `hello-server` from the `hello-app` container image, run the following `kubectl create` command:

```bash
kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
```

2. To create a Kubernetes Service, which is a Kubernetes resource that lets you expose your application to external traffic, run the following `kubectl expose` command:

```bash
kubectl expose deployment hello-server --type=LoadBalancer --port 8080
```

3. To inspect the `hello-server` Service, run `kubectl get`:

```bash
kubectl get service
```

4. To view the application from your web browser, open a new tab and enter the following address, replacing `[EXTERNAL IP]` with the `EXTERNAL-IP` for `hello-server`.

```text
http://[EXTERNAL-IP]:8080
```

### Delete the cluster

1. To **delete** the cluster, run the following command:

```bash
gcloud container clusters delete lab-cluster
```

2. When prompted, type `Y` and press **Enter** to confirm.

Deleting the cluster can take a few minutes.
