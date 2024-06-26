# Pre-work for IKC demo setup

This chapter describes the main pre-work steps which should be taken to prepare for IKC demo setup.

## 1. Prepare the data assets and connections

The IKC demo requires the following types of the assets to be used:

1. Files to import into Glossary - available in IBM Box folder for IBMers and Business Partners by request [using the following link](https://ibm.box.com/s/6vp8b2ooaldso8df0h0n8n1f2zku75qa).

> [!NOTE]
> Link is not broken. To access it, please contact me directly.

- Categories
- Business Terms
- Policies and Data Governance Rules
- Reference Data Sets
- Files with data for DQ tasks

2. External Databases

   - Databases with data for MDI, MDE, Profiling
   - Databases with data for DQ custom business rules demo
   - Databases with PII information for demonstration of the Data Privacy on the fly

3. DataStage Assets

   - Databases for Lineage demo
   - ETL jobs for the Lineage demo

4. Address Data Verification
   - Data libraries for the AVI demo
   - ETL jobs for the AVI

## 2. Deploy the CP4D cluster

There are several options on how that can be done
The external to IBM users may create their own cluster in:

- IBM cloud - paid service
- You own datacenter or hosted in AWS/Azure/GCP - with trial license obtained from your contact in IBM sales team
- IBM SaaS accounts on cloud.ibm.com - there are several ways to deploy services, for the basic hands-on experience some of the demo componets support even "light" unpaid version of the service you may order for immediate provisioning

IBMers and Business Partners can use Techzone accounts to request Base Installation clusters by this link: https://techzone.ibm.com/collection/5fb3200cec8dd00017c57f20

Currently the demo scenario is based on CP4D v4.8.2 and it may be updated later, so please select the latest available version on the page of Certified Base Images either with pre-deployed required list of services or provision them on your own

## 3. Provision required list of services

Depending on the selected demo scenario you will require one or more CP4D services deployed on the platform. The required combination of those is listed in the table below.

| Demo Block to Service Mapping   | Glossary | Data Profiling, MDI, MDE, Privacy | Lineage with Manta | ETL with DQ |
| :------------------------------ | :------: | :-------------------------------: | :----------------: | :---------: |
| IKC install_wkc_core_only: true |    V     |                 V                 |         V          |      V      |
| IKC enableKnowledgeGraph: true  |    V     |                 V                 |         -          |      -      |
| IKC enableDataQuality: true     |    -     |                 V                 |         -          |      V      |
| IKC enableMANTA: true           |    -     |                 -                 |         V          |      -      |
| DataStage Ent Plus              |    -     |    limited auto install by IKC    |         V          |      V      |
| Data Virtualization             |    -     |                 V                 |         -          |      -      |
| Manta                           |    -     |                 -                 |         V          |      -      |

## 4. Create the users/roles

The provisioning of CP4D always provides you the Admin user you may utilise right away. For some specific demo scenarios you would have to use more than one user account to demonstrate platform capabilities.

Cases include:

- Security on the Catalogs, Projects, Categories
- Security on Platform connections
- Data Masking/Data Privacy capabilites
- Workflows demonstration

If you plan to demonstrate those scenarios, use the IBM Cloud Pak for Data documentation to perform the proper setup of users, roles and permissions
