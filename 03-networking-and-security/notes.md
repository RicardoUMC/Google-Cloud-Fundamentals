# Google Cloud Networking and Security

<!--toc:start-->
- [Google Cloud Networking and Security](#google-cloud-networking-and-security)
  - [Networking](#networking)
    - [Content Delivery and Load Balancing Flow](#content-delivery-and-load-balancing-flow)
    - [Google Cloud Network Overview](#google-cloud-network-overview)
  - [Virtual Private Cloud (VPC)](#virtual-private-cloud-vpc)
    - [Key Features of Google Cloud VPC:](#key-features-of-google-cloud-vpc)
    - [Types of VPC Networks:](#types-of-vpc-networks)
  - [Public and Internal IP Addresses](#public-and-internal-ip-addresses)
    - [Public IPs](#public-ips)
    - [Internal IPs](#internal-ips)
  - [Google Cloud Network](#google-cloud-network)
  - [Routes and Firewall Rules](#routes-and-firewall-rules)
  - [Multiple VPC Networks](#multiple-vpc-networks)
    - [Multiple VPCs Lab](#multiple-vpcs-lab)
    - [Create the managementnet Network](#create-the-managementnet-network)
    - [Create the privatenet Network](#create-the-privatenet-network)
    - [Create Firewall Rules for Managementnet](#create-firewall-rules-for-managementnet)
    - [Creating Firewall Rules via Cloud Shell](#creating-firewall-rules-via-cloud-shell)
    - [Create a VM Instance](#create-a-vm-instance)
    - [Create she privatenet-vm-1 Instance](#create-she-privatenet-vm-1-instance)
    - [Explore VM Connectivity](#explore-vm-connectivity)
      - [Ping External IP Addresses](#ping-external-ip-addresses)
      - [Ping Internal IP Addresses](#ping-internal-ip-addresses)
    - [Create a VM Instance with Multiple Network Interfaces](#create-a-vm-instance-with-multiple-network-interfaces)
  - [Explore Network Interface Details](#explore-network-interface-details)
  - [Explore Network Connectivity](#explore-network-connectivity)
  - [Controlling VPC Network Access](#controlling-vpc-network-access)
    - [Create the web servers](#create-the-web-servers)
    - [Create the firewall rule](#create-the-firewall-rule)
    - [Explore the Network and Security Admin roles](#explore-the-network-and-security-admin-roles)
  - [Building a Hybrid Cloud](#building-a-hybrid-cloud)
    - [Options for Building a Hybrid Cloud](#options-for-building-a-hybrid-cloud)
  - [Load Balancing Options](#load-balancing-options)
    - [Key Features of Cloud Load Balancing:](#key-features-of-cloud-load-balancing)
    - [Types of Load Balancers:](#types-of-load-balancers)
  - [Application Load Balancing](#application-load-balancing)
    - [Configure HTTP and health check firewall rules](#configure-http-and-health-check-firewall-rules)
    - [Configure instance templates and create instance groups](#configure-instance-templates-and-create-instance-groups)
    - [Configure the application load balancer](#configure-the-application-load-balancer)
    - [Test the application load balancer](#test-the-application-load-balancer)
    - [Denylist the siege-vm](#denylist-the-siege-vm)
<!--toc:end-->

---

## Networking

Networking in Google Cloud refers to the backbone infrastructure that facilitates communication between different resources, services, and users. It includes Virtual Private Cloud (VPC), load balancing, hybrid connectivity, and security features to ensure reliable and secure data transfer. Google Cloud's networking solutions are designed to offer scalability, performance, and global reach.

---

### Content Delivery and Load Balancing Flow

1. **DNS Lookup**:
   - The web client (end user) performs a Domain Name System (DNS) lookup, which is served by **Cloud DNS**.

2. **Content Delivery Networks (CDNs)**:
   - CDNs are locations within the Google network used to cache content closer to users to improve application performance.

3. **Request Handling**:
   - The user requests a page.

4. **CDN Caching**:
   - If the CDN option was selected during Cloud Load Balancing configuration:
     - The request will go to the closest CDN location.
     - The CDN will serve the page directly if it has the content stored.

5. **Fallback to Load Balancing**:
   - If the CDN doesn’t have the requested page, it will forward the request to **Cloud Load Balancing**.

6. **Frontend Server Selection**:
   - Cloud Load Balancing picks an appropriate frontend server to serve the request.
   - The page is then returned to the user.

---

### Google Cloud Network Overview

- In Google Cloud, a **"network"** is an isolated global resource holding network configurations.

- Instances are deployed in **regional subnetworks**, but policies (firewall, routing, etc.) and access through **VPN** are configured at the global network level.

---

## Virtual Private Cloud (VPC)

A Virtual Private Cloud (VPC) is a secure, private cloud-computing model hosted within a public cloud, combining the scalability of public cloud computing with the data isolation of private cloud computing. VPC networks connect Google Cloud resources to each other and the internet, allowing segmentation, firewall rules, and static routes.

---

### Key Features of Google Cloud VPC:

- **Global Scope**: VPCs are global, with subnets spanning zones within a region.
- **Subnet Flexibility**: Subnet sizes can be expanded without affecting existing virtual machines.
- **Resilience**: Resources in different zones can reside on the same subnet, enabling robust yet simple network designs.

---

### Types of VPC Networks:

1. **Auto Mode**:
   - Automatically creates one subnet per region with predefined IP ranges and default firewall rules.
   - Limited flexibility, suited for isolated use cases like POCs and testing.
   - IP range expansion limited to a /16 prefix.

2. **Custom Mode**:
   - Requires manual subnet creation with user-defined IP ranges.
   - Provides complete control, suitable for production environments.
   - Subnet IP ranges cannot overlap.
   - Conversion from auto mode to custom mode is one-way.

A Custom Mode VPC offers greater flexibility and is ideal for production use cases, while Auto Mode VPCs are better for simpler, isolated scenarios.

---

## Public and Internal IP Addresses

A Virtual Private Cloud (VPC) consists of subnetworks (subnets), each configured with a private IP CIDR address. CIDR (Classless Inter-Domain Routing) determines the internal IP addresses used by virtual machines in the subnet. Internal IPs are used solely for communication within the VPC and are not routable to the internet.

IPv4 addresses are 32 bits long, segmented into octets. The CIDR suffix determines the number of available IP addresses (e.g., a /16 range provides 65,536 IPs, halving with each increment).

---

### Public IPs

- Can be **ephemeral** or **reserved**.
- Allocated from regional IP pools.
- Reserved IPs incur charges if not attached to a VM.
- VMs are unaware of their public IPs and display only internal IPs in their network configuration.

---

### Internal IPs

- Allocated via DHCP, with leases renewed every 24 hours.
- VM hostnames are associated with internal IPs via a network-scoped DNS service.

---

## Google Cloud Network

Google Cloud provides the following networking products:

- **Google Cloud VPC**: Offers a comprehensive set of networking capabilities to connect and isolate resources for security, compliance, and environment segregation (e.g., development vs. production).

- **Cloud Load Balancing**: Delivers high-performance, scalable load balancing to ensure consistent performance for users.

- **Cloud CDN**: A content delivery network that ensures high availability and performance by caching content close to users, leveraging Google’s global network for low-latency and cost-effective delivery.

- **Cloud Interconnect**: Connects on-premises infrastructure to Google’s network edge with enterprise-grade connections, often through Google’s partner network-service providers.

- **Cloud DNS**: Translates domain names to IP addresses using high-volume DNS services, suitable for production applications.

---

## Routes and Firewall Rules

VPCs use routing tables to forward traffic within the same network, across subnetworks, or between Google Cloud zones without requiring external IPs. These built-in tables list routes to network destinations and ensure efficient traffic flow.

VPCs also provide a global distributed firewall to control access to instances. Firewall rules can be defined using metadata tags on Compute Engine instances. For example, tagging VMs with "WEB" allows you to create a rule permitting traffic on ports 80 and 443 to all instances with that tag.

---

## Multiple VPC Networks

To build robust networking solutions across multiple projects, VPCs can communicate using:

1. **VPC Peering**:
   - Establishes private RFC 1918 connectivity between two VPCs, even across different projects or organizations.
   - Maintains decentralized control with separate firewall rules and routing tables for each VPC.
   - Avoids latency, security, and cost issues associated with external IPs or VPNs.

2. **Shared VPC**:
   - Connects resources from multiple projects to a common VPC network.
   - Uses internal IPs for secure and efficient communication.
   - Designates a host project to manage the shared VPC, with other projects attached as service projects.

---

### Multiple VPCs Lab

Create custom mode VPC networks with firewall rules
Create two custom networks: `managementnet` and `privatenet`, along with firewall rules to allow SSH, ICMP, and RDP ingress traffic.

---

### Create the managementnet Network

1. Navigate to the **Cloud Console**:
   - Go to **Navigation menu > VPC network > VPC networks**.

2. Click **Create VPC Network**.

3. Set the following properties:
   - **Name**: `managementnet`
   - **Subnet creation mode**: `Custom`

4. Configure the subnet:
   - **Name**: `managementsubnet-1`
   - **Region**: `us-central1`
   - **IPv4 range**: `10.130.0.0/20`

5. Click **Done**.

6. Click **Close**.

7. Click **Create**.

---

### Create the privatenet Network

1. **Create the privatenet network**:

   ```bash
   gcloud compute networks create privatenet --subnet-mode=custom
   ```

2. **Create the privatesubnet-1 subnet**:

   ```bash
   gcloud compute networks subnets create privatesubnet-1 --network=privatenet --region=us-central1 --range=172.16.0.0/24
   ```

3. **Create the privatesubnet-2 subnet**:

   ```bash
   gcloud compute networks subnets create privatesubnet-2 --network=privatenet --region=asia-southeast1 --range=172.20.0.0/20
   ```

4. **List the available VPC networks**:

   ```bash
   gcloud compute networks list
   ```

   - The output should display the default, managementnet, and privatenet networks, with their respective modes and configurations.

5. **List the available VPC subnets (sorted by VPC network)**:

   ```bash
   gcloud compute networks subnets list --sort-by=NETWORK
   ```

   - The output will show subnets for each network, including the custom subnets created for managementnet and privatenet.

---

### Create Firewall Rules for Managementnet

1. **Navigate to Firewall Settings in the Cloud Console**:
   - Go to **Navigation menu > VPC network > Firewall**.

2. **Create a New Firewall Rule**:
   - Click **+ Create Firewall Rule**.

3. **Set Basic Rule Properties**:
   - **Name**: Provide a unique name (e.g., `managementnet-allow-icmp-ssh-rdp`).
   - **Network**: Select the target VPC network (e.g., `managementnet`).
   - **Targets**: Choose the instances to apply the rule (e.g., `All instances in the network`).

4. **Define Source and Protocols**:
   - **Source filter**: Select `IPv4 Ranges`.
   - **Source IPv4 ranges**: Enter the desired range (e.g., `0.0.0.0/0`).
   - **Protocols and ports**: Specify allowed protocols and ports:
     - Check **tcp**, type `22, 3389`.
     - Check **Other protocols**, type `icmp`.

5. **Generate Equivalent Command Line (Optional)**:
   - Click **EQUIVALENT COMMAND LINE** to view the corresponding gcloud command.

6. **Finalize Creation**:
   - Click **Close** to exit the command line preview.
   - Click **Create** to save the rule.

---

### Creating Firewall Rules via Cloud Shell

1. **Open Cloud Shell**:
   - Click the **Cloud Shell** icon in the Google Cloud Console.

2. **Run the Command to Create a Firewall Rule**:
   - Use the following command format:
     ```bash
     gcloud compute firewall-rules create RULE_NAME \
     --direction=INGRESS \
     --priority=1000 \
     --network=NETWORK_NAME \
     --action=ALLOW \
     --rules=PROTOCOLS_AND_PORTS \
     --source-ranges=SOURCE_RANGES
     ```
   - Example for `privatenet`:
     ```bash
     gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp \
     --direction=INGRESS \
     --priority=1000 \
     --network=privatenet \
     --action=ALLOW \
     --rules=icmp,tcp:22,tcp:3389 \
     --source-ranges=0.0.0.0/0
     ```

3. **Verify the Creation**:
   - Run the following command to list all firewall rules, sorted by the VPC network:
     ```bash
     gcloud compute firewall-rules list --sort-by=NETWORK
     ```

4. **Review the Output**:
   - Ensure the newly created rule is listed with the correct properties.

> [!NOTE]
>
> - Multiple protocols and ports can be combined in a single rule or spread across multiple rules.
> - Firewall rules can be managed through the Cloud Console or Cloud Shell for flexibility.

---

### Create a VM Instance

1. **Open Cloud Console**:
   - Navigate to **Navigation menu > Compute Engine > VM instances**.

2. **Create a New Instance**:
   - Click **Create Instance**.

3. **Configure Machine Settings**:
   - Set the following values, leaving all other values at their defaults:
     - **Name**: `managementnet-vm-1`
     - **Region**: `us-central1`
     - **Zone**: `us-central1-b`
     - **Series**: `E2`
     - **Machine Type**: `e2-micro`

4. **Configure Networking Settings**:
   - Click on the dropdown to edit **Network interfaces**.
   - Set the following values:
     - **Network**: `managementnet`
     - **Subnetwork**: `managementsubnet-1`
   - Click **Done**.

5. **Create the Instance**:
   - Click **Create** to finalize the creation of the `managementnet-vm-1` instance.

---

### Create she privatenet-vm-1 Instance

1. **Create the Instance Using Cloud Shell**:

   ```bash
   gcloud compute instances create privatenet-vm-1 \
       --zone=<ZONE> \
       --machine-type=e2-micro \
       --subnet=privatesubnet-1
   ```

2. **Expected Output**:

   ```
   Created [https://www.googleapis.com/compute/v1/projects/<PROJECT_ID>/zones/<ZONE>/instances/privatenet-vm-1].
   NAME: privatenet-vm-1
   ZONE: <ZONE>
   MACHINE_TYPE: e2-micro
   INTERNAL_IP: 172.16.0.2
   EXTERNAL_IP: <EXTERNAL_IP>
   STATUS: RUNNING
   ```

3. **Verify Progress**:
   - Click **Check my progress** to verify the creation of the VM instance.

4. **List All VM Instances**:
   ```bash
   gcloud compute instances list --sort-by=ZONE
   ```

---

### Explore VM Connectivity

#### Ping External IP Addresses

1. **Navigate to VM Instances**:
   - Open **Navigation menu > Compute Engine > VM instances**.
   - Note the external IPs of the desired VMs.

2. **Test Connectivity**:
   - SSH into a VM (e.g., `mynet-vm-1`) and run:
     ```bash
     ping -c 3 <EXTERNAL_IP>
     ```
   - Repeat for each instance.

---

#### Ping Internal IP Addresses

1. **Note Internal IPs**:
   - Navigate to **Compute Engine > VM instances** and note the internal IPs.

2. **Test Connectivity**:
   - SSH into a VM (e.g., `mynet-vm-1`) and run:
     ```bash
     ping -c 3 <INTERNAL_IP>
     ```
   - Observe packet loss to determine connectivity:
     - Same VPC network: Should work.
     - Different VPC networks: Will not work unless VPC peering or VPN is configured.

---

### Create a VM Instance with Multiple Network Interfaces

1. **Create the VM**:
   - Navigate to **Compute Engine > VM instances** and click **Create Instance**.
   - Configure the following:
     - **Name**: `vm-appliance`
     - **Region**: `<US_Region>`
     - **Zone**: `<US_Zone>`
     - **Series**: `E2`
     - **Machine Type**: `e2-standard-4`

2. **Add Network Interfaces**:
   - **Interface 1**:
     - **Network**: `privatenet`
     - **Subnetwork**: `privatesubnet-1`
   - **Interface 2**:
     - **Network**: `managementnet`
     - **Subnetwork**: `managementsubnet-1`
   - **Interface 3**:
     - **Network**: `mynetwork`
     - **Subnetwork**: `mynetwork`

3. **Finalize**:
   - Click **Create**.

---

## Explore Network Interface Details

1. **Interface Overview**:
   - Navigate to **Compute Engine > VM instances** and click on the VM.
   - Select each interface (e.g., `nic0`, `nic1`) to view details.

2. **Verify Internal IP Assignments**:
   - Confirm that each network interface is assigned an IP within its respective subnet.

3. **List Network Interfaces via SSH**:
   ```bash
   sudo ifconfig
   ```

---

## Explore Network Connectivity

1. **Ping Other Instances**:
   - SSH into the VM with multiple interfaces (e.g., `vm-appliance`).
   - Test connectivity to instances in different subnets:
     ```bash
     ping -c 3 <INTERNAL_IP>
     ```

2. **Resolve Hostnames**:
   - Use DNS instead of IPs for VMs in the same VPC:
     ```bash
     ping -c 3 <INSTANCE_NAME>
     ```

3. **List Routes**:

   ```bash
   ip route
   ```

   - Verify that traffic to other subnets is routed via the appropriate interface.

> [!NOTE]
>
> - Internal communication between VMs in different VPCs requires additional configuration (e.g., VPC peering or VPN).
> - By default, traffic leaving a VM uses the primary interface (`eth0`) unless manually configured.

---

## Controlling VPC Network Access

In the real-world you need to protect sensitive data and ensure the continued availability of your web applications at all times. Learn how to use the Google Cloud VPC network to create a more secure, scalable, and manageable web server deployment within your Google Cloud environment.

---

### Create the web servers

1. **Create the blue server**
   - Navigate to **Compute Engine > VM instances**.
   - Click **Create Instance**.
     - **Name**: `blue`
     - **Network tags**: `web-server`
   - Click **Create**.

2. **Create the green server**
   - Navigate to **Compute Engine > VM instances**.
   - Click **Create Instance**.
     - **Name**: `green`
   - Click **Create**.

3. **Install nginx and customize welcome pages**
   - SSH into `blue` and `green`.
   - Install nginx:
     ```bash
     sudo apt-get install nginx-light -y
     ```
   - Edit the welcome page:

     ```bash
     sudo nano /var/www/html/index.nginx-debian.html
     ```

     - Replace `<h1>Welcome to nginx!</h1>` with:
       - `blue`: `<h1>Welcome to the blue server!</h1>`
       - `green`: `<h1>Welcome to the green server!</h1>`

   - Save and verify:
     ```bash
     cat /var/www/html/index.nginx-debian.html
     ```

---

### Create the firewall rule

1. **Create the tagged firewall rule**
   - Navigate to **VPC network > Firewall** and click **Create Firewall Rule**.
     - **Name**: `allow-http-web-server`
     - **Network**: `default`
     - **Target tags**: `web-server`
     - **Source IPv4 ranges**: `0.0.0.0/0`
     - **Protocols and ports**:
       - `tcp:80`
       - `icmp`
   - Click **Create**.

2. **Create a test-vm**
   - Open **Cloud Shell** and run:
     ```bash
     gcloud compute instances create test-vm --machine-type=e2-micro --subnet=default --zone=ZONE
     ```

3. **Test HTTP connectivity**
   - SSH into `test-vm` and test connectivity:
     ```bash
     curl <blue_internal_IP>
     curl <green_internal_IP>
     curl <blue_external_IP>
     curl <green_external_IP>
     ```

---

### Explore the Network and Security Admin roles

1. **Verify current permissions**
   - SSH into `test-vm` and try:
     ```bash
     gcloud compute firewall-rules list
     gcloud compute firewall-rules delete allow-http-web-server
     ```

2. **Create a service account**
   - Navigate to **IAM & admin > Service Accounts**.
   - Create a service account:
     - **Name**: `Network-admin`
     - **Role**: `Compute Network Admin`
   - Download the JSON key and rename it to `credentials.json`.
   - Upload to `test-vm` and authenticate:
     ```bash
     gcloud auth activate-service-account --key-file credentials.json
     ```

3. **Update service account to Security Admin**
   - Navigate to **IAM & admin > IAM**.
   - Update `Network-admin` role to `Compute Security Admin`.
   - Verify and delete the firewall rule:
     ```bash
     gcloud compute firewall-rules list
     gcloud compute firewall-rules delete allow-http-web-server
     ```

---

## Building a Hybrid Cloud

Many Google Cloud customers want to connect their Google Virtual Private Clouds to other networks in their system, such as on-premises networks or networks in other clouds.

---

### Options for Building a Hybrid Cloud

Each option offers different levels of security, performance, and reliability to match varying business needs:

- **VPN Connection**: Start with a virtual private network connection over the internet using the **IPsec** VPN protocol to create a secure "tunnel" connection.

- **Cloud Router with Dynamic Routing**: This option uses **Cloud Router** to exchange route information between networks over the VPN. It supports dynamic routing via **Border Gateway Protocol (BGP)** and automatically propagates routes for new subnets added to the Google VPC.

- **Direct Peering**: Involves placing a router in the same public data center as a Google point of presence (PoP) to exchange traffic directly between networks using **Direct Peering**. With over 100 Google PoPs available globally, it also supports **Carrier Peering**, which connects to Google Workspace and Google Cloud products through public IPs.

- **Dedicated Interconnect**: Establishes direct, private connections to Google, configurable to meet topologies covered by a **99.99% SLA**. It also supports backup connections with VPN for enhanced reliability.

- **Partner Interconnect**: Utilizes a supported service provider to connect on-premises networks and a Google VPC network. It is suitable for data centers that cannot reach a Dedicated Interconnect colocation facility and supports both mission-critical services (99.99% SLA) and applications that can tolerate some downtime. However, Google is not responsible for issues outside its network or with the service provider.

---

## Load Balancing Options

Cloud Load Balancing is a fully distributed, software-defined, managed service for handling all your traffic. It eliminates the need for scaling or managing load balancers as they do not run on VMs.

---

### Key Features of Cloud Load Balancing:

- **Traffic Types**: Supports HTTP(S), other TCP and SSL traffic, and UDP traffic.
- **Cross-Region Load Balancing**: Includes automatic failover and adapts to backend health, ensuring minimal disruption.
- **No Pre-Warming Required**: Automatically handles traffic spikes without advance notice.

---

### Types of Load Balancers:

1. **Application Load Balancers** (Layer 7):
   - Works at the application layer of the OSI model.
   - Ideal for HTTP(S) headers, cookies, or URL path-based routing.
   - Features include SSL/TLS termination, session affinity, and content-based routing.

2. **Network Load Balancers** (Layer 4):
   - Operates at the network layer of the OSI model.
   - Suitable for IP address and port-based routing.
   - Used for TCP/UDP traffic with a focus on low latency and high throughput.
   - Supports TCP/UDP load balancing and health checks.

---

## Application Load Balancing

### Configure HTTP and health check firewall rules

1. Navigate to **VPC network > Firewall** in the Cloud Console.

2. Click **Create Firewall Rule**.

3. Set the following values for the HTTP firewall rule:
   - **Name**: `default-allow-http`
   - **Network**: `default`
   - **Targets**: `specified target tags`
   - **Target tags**: `http-server`
   - **Source filter**: `IPv4 ranges`
   - **Source IPv4 ranges**: `0.0.0.0/0`
   - **Protocols and ports**: `tcp`, type `80`

4. Click **Create** to finalize the HTTP firewall rule.

5. In the same page, click **Create Firewall Rule** again.

6. Set the following values for the health check firewall rule:
   - **Name**: `default-allow-health-check`
   - **Network**: `default`
   - **Targets**: `specified target tags`
   - **Target tags**: `http-server`
   - **Source filter**: `IPv4 ranges`
   - **Source IPv4 ranges**: `130.211.0.0/22, 35.191.0.0/16`
   - **Protocols and ports**: `tcp`

7. Click **Create** to finalize the health check firewall rule.

---

### Configure instance templates and create instance groups

1. Navigate to **Compute Engine > Instance Templates**.

2. Click **Create Instance Template**.

3. Configure the following for Region 1 template:
   - **Name**: `region-1-template`
   - **Location**: `Global`
   - **Series**: `E2`
   - **Machine Type**: `e2-micro`
   - **Network tags**: `http-server`
   - **Network**: `default`
   - **Subnetwork**: `default region 1`
   - **Startup-script-url**: `gs://cloud-training/gcpnet/httplb/startup.sh`
4. Click **Create**.

5. Repeat the steps to create Region 2 template with the following changes:
   - **Name**: `region-2-template`
   - **Subnetwork**: `default region 2`.

6. Navigate to **Compute Engine > Instance Groups**.

7. Click **Create Instance Group**.

8. Configure the following for Region 1 MIG:
   - **Name**: `region-1-mig`
   - **Instance template**: `region-1-template`
   - **Location**: `Multiple zones`
   - **Region**: `Region 1`
   - **Minimum instances**: `1`
   - **Maximum instances**: `2`
   - **Autoscaling signal**: `CPU utilization`
   - **Target CPU utilization**: `80`
   - **Initialization period**: `45 seconds`
9. Click **Create**.

10. Repeat the steps to create Region 2 MIG with the following changes:
    - **Name**: `region-2-mig`
    - **Instance template**: `region-2-template`
    - **Region**: `Region 2`.

---

### Configure the application load balancer

1. Navigate to **Networking > Network Services > Load Balancing**.

2. Click **Create Load Balancer**.

3. Select **Application Load Balancer HTTP(S)** and proceed.

4. Choose **Public facing (external)** and **Best for global workloads**.

5. Set **Load Balancer Name** to `http-lb`.

6. Click **Frontend Configuration**.
   - Set the following:
     - **Protocol**: `HTTP`
     - **IP version**: `IPv4`
     - **IP address**: `Ephemeral`
     - **Port**: `80`
   - Click **Done**.

7. Add another frontend IP for **IPv6**:
   - **IP version**: `IPv6`
   - **IP address**: `Auto-allocate`.
   - Click **Done**.

8. Click **Backend Configuration**.
   - Create a backend service with:
     - **Name**: `http-backend`
     - **Instance group**: `region-1-mig`
     - **Port numbers**: `80`
     - **Balancing mode**: `Rate`
     - **Maximum RPS**: `50`
   - Add another backend for `region-2-mig`:
     - **Balancing mode**: `Utilization`
     - **Maximum backend utilization**: `80%`.
   - Click **Done**.

9. Create a health check:
   - Set the following:
     - **Name**: `http-health-check`
     - **Protocol**: `TCP`
     - **Port**: `80`
   - Enable logging and set **Sample rate** to `1`.
   - Click **Done**.

10. Review the backend and frontend configurations.

11. Click **Create** to finalize the load balancer setup.

---

### Test the application load balancer

1. Note the IPv4 and IPv6 addresses of the load balancer.

2. Open a browser and navigate to `http://[lb_ip_v4]` or `http://[lb_ip_v6]`.

3. Verify that the traffic is routed to the nearest backend.

4. Create a VM named `siege-vm` in Region 3.
   - Navigate to **Compute Engine > VM instances** and click **Create Instance**.
   - Configure the following:
     - **Name**: `siege-vm`
     - **Region**: `Region 3`
     - **Zone**: Select a zone in Region 3
     - **Machine Type**: `e2-micro`
   - Click **Create**.

5. SSH into the `siege-vm` instance.
   - Navigate to **Compute Engine > VM instances**, and click **SSH** next to the `siege-vm` instance.

6. Install `siege` in the `siege-vm` instance:
   ```bash
   sudo apt-get update
   sudo apt-get install siege -y
   ```

7. Set the load balancer IP address as an environment variable:
   ```bash
   export lb_ip=[LOAD_BALANCER_IP]
   ```

8. Run a load test on the load balancer:
   ```bash
   siege -c 150 -t120s http://$lb_ip
   ```

9. Monitor traffic distribution:
   - Go to **Load Balancing > Monitoring Tab** in the Google Cloud Console.
   - Verify that traffic is distributed across the backend instances as expected.

---

### Denylist the siege-vm

1. Note the external IP of `siege-vm`.

2. Navigate to **Network Security > Cloud Armor Policies** in the Google Cloud Console.

3. Create a new policy:
   - Click **Create Policy**.
   - **Name**: `denylist-siege`.
   - **Default rule action**: `Allow`.
   - Click **Create** to save the policy.

4. Add a denylist rule to the policy:
   - Select the newly created `denylist-siege` policy.
   - Click **Add Rule**.
   - Set the following properties:
     - **Condition**: Match `siege-vm IP`.
     - **Action**: `Deny`.
     - **Response code**: `403`.
     - **Priority**: `1000`.
   - Click **Save Rule**.

5. Attach the policy to the target backend service:
   - Navigate to **Target** in the `denylist-siege` policy.
   - Click **Edit Target**.
   - **Type**: `Backend service`.
   - **Target**: Select `http-backend`.
   - Click **Save** to apply the policy.

6. Verify the denylist configuration:
   - SSH into the `siege-vm` instance.
   - Attempt to access the load balancer:
     ```bash
     curl http://[LOAD_BALANCER_IP]
     ```
   - Confirm that the response is denied with a `403 Forbidden` error.
