## Introduction

High Performance Computing is changing product development and research enabling customers to solve complex problems faster. This means fewer prototypes, accelerates testing, and decreases time to market. Oracle offers on-demand HPC infrastructure, suitable for any HPC workload, based on the most advanced compute, storage, networking, and software technologies. You get all this at a fraction of the cost of building it yourself and avoid capacity utilization issues.

Oracle provides the most elastic and scalable cloud infrastructure to run your HPC applications. With virtually unlimited capacity, engineers, researchers, and HPC system owners can innovate beyond the limitations of on-premises HPC infrastructure. Oracle delivers an integrated suite of services that provides everything needed to quickly and easily build and manage HPC clusters in the cloud to run the most compute intensive workloads across various industry verticals. These workloads span the traditional HPC applications, like genomics, computational chemistry, financial risk modeling, computer aided engineering, seismic imaging, and weather prediction, as well as emerging applications, like machine learning, deep learning, and autonomous driving.

With Oracle Cloud Infrastructure, businesses can run performance intensive HPC workloads requiring millions of IOPs, millisecond latency, and many GB/s of bandwidth, on a pay per-use or non- metered model, saving 32% on a 3-year TCO.

These hands-on lab guides provide step-by-step directions to setting up and using your High Performance Computing platform in the Oracle Cloud Infrastructure.

Lab 1 deals with setting up the High Performance Compute Instance in the Oracle Cloud Infrastructure.

Labs 2 are geared towards Managing your High Performance Compute Instance using OCI CLI tools.

Labs 3 designed to assist in the assessment of the OpenFOAM CFD Software in Oracle Cloud Infrastructure.



## Goals for this workshop
1. Prepare your private network in the Oracle Cloud Infrastructure
2. Provision High Performance Compute Instance in a private OCI network
3. Configure a development system for use with your High Performance Compute Instance
4. Use OCI CLI commands to work with High Performance Compute Instance
5. Automation with Terraform
6. Run OpenFoam projects with your High Performance Compute Instance



# Workshop Overview

## Before You Begin
**What is High Performance Computing?**

- HPC, or supercomputing, is like everyday computing, only more powerful. It is a way of processing huge volumes of data at very high speeds using multiple computers and storage devices as a cohesive fabric. HPC makes it possible to explore and find answers to some of the world’s biggest problems in science, engineering, and business.

- A cluster network is a pool of high performance computing (HPC) instances that are connected with a high-bandwidth, ultra low-latency network. Each node in the cluster is a bare metal machine located in close physical proximity to the other nodes. A remote direct memory access (RDMA) network between nodes provides latency as low as single-digit microseconds, comparable to on-premises HPC clusters.

- High Performance Computing is offered on Oracle Cloud Infrastructure, within OCI regions.
- High Performance Computing Instance available in Oracle MarketPlace Image and BM.HPC2.36 in OCI.
- High Performance Computing rack in Oracle MarketPlace Image includes HPC cluster nodes, cluster network and NFS share.
- The compute nodes are connected via cluster network that provides RDMA based storage access to the compute nodes.
- Currently, a single BM per compute node is supported. It allows root access for customers
while protecting hardware and network, compute nodes are virtualized using BM.HPC2.36.


**You are all set, let's begin!**


## Lab 1: Provision HPC Cluster from Oracle MarketPlace Image

**Key Objectives**:
- Create 2 HPC cluster compute nodes 
- Layout a secure cluster network for the HPC nodes and bastion nodes
- Create NFS share between custer nodes



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
