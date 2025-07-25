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
  - [Building a Hybrid Cloud](#building-a-hybrid-cloud)
  - [Load Balancing Options](#load-balancing-options)
<!--toc:end-->

## Networking

Networking in Google Cloud refers to the backbone infrastructure that facilitates communication between different resources, services, and users. It includes Virtual Private Cloud (VPC), load balancing, hybrid connectivity, and security features to ensure reliable and secure data transfer. Google Cloud's networking solutions are designed to offer scalability, performance, and global reach.

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

### Google Cloud Network Overview

- In Google Cloud, a **"network"** is an isolated global resource holding network configurations.

- Instances are deployed in **regional subnetworks**, but policies (firewall, routing, etc.) and access through **VPN** are configured at the global network level.

## Virtual Private Cloud (VPC)

A Virtual Private Cloud (VPC) is a secure, private cloud-computing model hosted within a public cloud, combining the scalability of public cloud computing with the data isolation of private cloud computing. VPC networks connect Google Cloud resources to each other and the internet, allowing segmentation, firewall rules, and static routes.

### Key Features of Google Cloud VPC:

- **Global Scope**: VPCs are global, with subnets spanning zones within a region.
- **Subnet Flexibility**: Subnet sizes can be expanded without affecting existing virtual machines.
- **Resilience**: Resources in different zones can reside on the same subnet, enabling robust yet simple network designs.

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

## Public and Internal IP Addresses

A Virtual Private Cloud (VPC) consists of subnetworks (subnets), each configured with a private IP CIDR address. CIDR (Classless Inter-Domain Routing) determines the internal IP addresses used by virtual machines in the subnet. Internal IPs are used solely for communication within the VPC and are not routable to the internet.

IPv4 addresses are 32 bits long, segmented into octets. The CIDR suffix determines the number of available IP addresses (e.g., a /16 range provides 65,536 IPs, halving with each increment).

### Public IPs

- Can be **ephemeral** or **reserved**.
- Allocated from regional IP pools.
- Reserved IPs incur charges if not attached to a VM.
- VMs are unaware of their public IPs and display only internal IPs in their network configuration.

### Internal IPs

- Allocated via DHCP, with leases renewed every 24 hours.
- VM hostnames are associated with internal IPs via a network-scoped DNS service.

## Google Cloud Network

Google Cloud provides the following networking products:

- **Google Cloud VPC**: Offers a comprehensive set of networking capabilities to connect and isolate resources for security, compliance, and environment segregation (e.g., development vs. production).

- **Cloud Load Balancing**: Delivers high-performance, scalable load balancing to ensure consistent performance for users.

- **Cloud CDN**: A content delivery network that ensures high availability and performance by caching content close to users, leveraging Google’s global network for low-latency and cost-effective delivery.

- **Cloud Interconnect**: Connects on-premises infrastructure to Google’s network edge with enterprise-grade connections, often through Google’s partner network-service providers.

- **Cloud DNS**: Translates domain names to IP addresses using high-volume DNS services, suitable for production applications.

## Routes and Firewall Rules

VPCs use routing tables to forward traffic within the same network, across subnetworks, or between Google Cloud zones without requiring external IPs. These built-in tables list routes to network destinations and ensure efficient traffic flow.

VPCs also provide a global distributed firewall to control access to instances. Firewall rules can be defined using metadata tags on Compute Engine instances. For example, tagging VMs with "WEB" allows you to create a rule permitting traffic on ports 80 and 443 to all instances with that tag.

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

## Building a Hybrid Cloud

## Load Balancing Options

