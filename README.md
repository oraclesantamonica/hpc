## Overview

High Performance Computing is changing product development and research enabling customers to solve complex problems faster. This means fewer prototypes, accelerates testing, and decreases time to market. Oracle offers on-demand HPC infrastructure, suitable for any HPC workload, based on the most advanced compute, storage, networking, and software technologies. You get all this at a fraction of the cost of building it yourself and avoid capacity utilization issues.

Oracle provides the most elastic and scalable cloud infrastructure to run your HPC applications. With virtually unlimited capacity, engineers, researchers, and HPC system owners can innovate beyond the limitations of on-premises HPC infrastructure. Oracle delivers an integrated suite of services that provides everything needed to quickly and easily build and manage HPC clusters in the cloud to run the most compute intensive workloads across various industry verticals. These workloads span the traditional HPC applications, like genomics, computational chemistry, financial risk modeling, computer aided engineering, seismic imaging, and weather prediction, as well as emerging applications, like machine learning, deep learning, and autonomous driving.

With Oracle Cloud Infrastructure, businesses can run performance intensive HPC workloads requiring millions of IOPs, millisecond latency, and many GB/s of bandwidth, on a pay per-use or non- metered model, saving 32% on a 3-year TCO.

These hands-on lab guides provide step-by-step directions to setting up and using your High Performance Computing platform in the Oracle Cloud Infrastructure.

Lab 1 deals with setting up the High Performance Compute Instance in the Oracle Cloud Infrastructure.

Labs 2 are geared towards Managing your High Performance Compute Instance using OCI CLI tools.

Labs 3 designed to assist in the assessment of the OpenFOAM CFD Software in Oracle Cloud Infrastructure.



## Goals for this workshop
1. Prepare your private network in the Oracle Cloud Infrastructure
2. Provision Exadata Cloud Service Infrastructure in a private OCI network
3. Provision databases on your Exadata Cloud Service Infrastructure
4. Configure a development system for use with your Exadata Cloud Service database
5. Setup VPN Connectivity to your Exadata Cloud Service Infrastructure
6. Setup, Discover, Manage and Monitor database with Enterprise Manager
7. Data Safe with Exadata Cloud Service
8. Migrate an on-prem application schema using Data Pump
9. Real time migration of database using Oracle Goldengate Replication
10. Backup and Recovery using Console and API's
11. Protect your data with Database Vault
12. Data Safe Advanced lab
13. Use OCI CLI commands to work with Exadata Cloud Service
14. Automation with Terraform
15. Build and deploy Python application stacks on Exadata Cloud Service
16. Build APEX application on Exadata Cloud Service
17. Install and configure Swingbench to simulate a transaction processing workload
18. build a docker container running node.js connected to EXACS database


# Workshop Overview

## Before You Begin
**What is Exadata Cloud Service?**

- Exadata Cloud Service is offered on Oracle Cloud Infrastructure, within OCI regions.
- Exadata Cloud Service available in quarter Rack, Half Rack or full Rack configurations.
- Exadata rack in OCI includes DB nodes, storage nodes and Infiniband switches.
- The storage and compute nodes are connected via high bandwidth Infiniband network that
provides RDMA based storage access to the compute nodes.
- Exadata storage software runs on storage servers and offloads database SQL processing
overheads.
- Currently, a single VM per compute node is supported. It allows root access for customers
while protecting hardware and network, DB nodes are virtualized using Xen based OVM.
- Oracle Manages storage cells, switches, management or IB network while customer manages
database compute nodes.
- Exadata Cloud Service provides a control Plane, a Web-based self-service management
interface for Exadata cloud service provisioning and interactive access to service administration function

**You are all set, let's begin!**


## Lab 1: Prepare your private network in the Oracle Cloud Infrastructure (demo only)

**Key Objectives**:
- Create compartments and user groups with the right set of access policies for separation of duties
- Create admin and database user accounts
- Layout a secure network for the database and application infrastructure


## Lab 2: Provision Exadata Infrastructure in a private OCI network (demo only)

**Key Objectives**:

As a fleet administrator,
- Deploy an Exadata Cloud Service Infrastructure in a pre-provisioned private network in your OCI account


## Lab 3: Provision databases on your Exadata Cloud Service Infrastructure (demo only)

**Key Objectives**:

As a database administrator,
- Deploy database onto an Exadata Cloud Service Infrastructure


## Appendix

**This covers some general help for Windows users and other occasional issues you may encounter while working with your Exadata Cloud Service.**
