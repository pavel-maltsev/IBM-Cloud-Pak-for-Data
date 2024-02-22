# 1. Objectives

The goal of this excersise is to demonstrate the capabilities of IBM Cloud Pak for Data on monitoring the data lineage, discovery of the internal pipelines in databases and external to then pipelines of DataStage ETL jobs running on IBM CP4D by using the IBM Manta software service.

The lab will use 2 databases and 2 ETL jobs to demonstrate the value of data lineage with Manta.

# 2. Pre-work

## 2.1 Data sources

Before starting the lab excersise please prepare 2 RDBMS data sources. Here we are to use the Postgres available databases deployed outside of CP4D cluster but which CP4D is able to natively connect to.

I've used the DVD Rental Database described in section [Sample data assets](/Setup%20WKC%20demo%20environment/Data%20Assets/Sample_data_assets.md). The database has been deployed twice on different schemas to serve as the source and as a target for the pipelines lineage.

## 2.2 Services used in the lab

For successful lab execution CP4D instance should have IKC, Manta and DataStage Ent. services deployed

## 2.3 Environment setup

### User creation

We would separate this demo scenario from the other topics and also demonstrate how the Connectors can be freely shared by the authorised user with other Data Stewards

For that we would need the different user to be created in CP4D main menu - Administration - Access Control

![alt text](/Data%20Lineage/images/user_create-0.png)

Click "Add users" in the right-top corner

![alt text](/Data%20Lineage/images/user_create-1.png)

I will create John Lineage here

![alt text](/Data%20Lineage/images/user_create-2.png)

For Platform assess select "Assign roles directly" and click Next

![alt text](/Data%20Lineage/images/user_create-3.png)

![alt text](/Data%20Lineage/images/user_create-4.png)

Provide the user Role of Data Steward and click Next again

![alt text](/Data%20Lineage/images/user_create-5.png)

![alt text](/Data%20Lineage/images/user_create-4.png)

Browse the summary and click Add

![alt text](/Data%20Lineage/images/user_create-6.png)

You will have now at least 2 users avaialble for this demo

![alt text](/Data%20Lineage/images/user_create-7.png)

### Create Platform Connections

In this chapter the Platform connections are to be created for the source and target Postgres databases.

Follow the CP4D main menu - Data - Platform connections menu

![alt text](/Data%20Lineage/images/pl_conn-0.png)

On the right side click "New connection" button

![alt text](/Data%20Lineage/images/pl_conn-1.png)

The revealed screen form shows you the variety of connections available on the CP4D platform. Those can be created in a centralised way and then re-used in several activities. The top-right link to "Supported connection types" will provide you documentation on connectivity options and applicability of those for specific functional areas.

Use the "Find connectors" line on top to search for "Postgre".

![alt text](/Data%20Lineage/images/pl_conn-2.png)

Click the one named PostgreSQL and then click Select.

![alt text](/Data%20Lineage/images/pl_conn-3.png)

Provide the Name "PostgreSQL_lineage_source" and other parameters for the database you've created on the earlier steps of the pre-work.

When entering the Credentials data choose the radio button for "Credential setting" as "Shared"

![alt text](/Data%20Lineage/images/pl_conn-4.png)

When done, press "Test connection" on the top-right of the page

![alt text](/Data%20Lineage/images/pl_conn-5.png)

You should receive confirmation on successful results

![alt text](/Data%20Lineage/images/pl_conn-6.png)

Click "Create" and the bottom-right

![alt text](/Data%20Lineage/images/pl_conn-7.png)

Done with Source database connection for Lineage lab. Next please repeate the same steps to create the connection for the Target database

Name the connection PostgreSQL_lineage_target and validate the connection works

![alt text](/Data%20Lineage/images/pl_conn.png)

Done with Target database connection creation. You should now see 2 available connections for PostgreSQL in the list of Platform connections

![alt text](/Data%20Lineage/images/pl_conn.png)
