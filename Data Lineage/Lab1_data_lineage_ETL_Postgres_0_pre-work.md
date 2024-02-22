# 1. Objectives

The goal of this excersise is to demonstrate the capabilities of IBM Cloud Pak for Data on monitoring the data lineage, discovery of the internal pipelines in databases and external to then pipelines of DataStage ETL jobs running on IBM CP4D by using the IBM Manta software service.

The lab will use 2 databases and 2 ETL jobs to demonstrate the value of data lineage with Manta.

# 2. Pre-work

## 2.1 Data sources

Before starting the lab excersise please prepare 2 RDBMS data sources. Here we are to use the Postgres available databases deployed outside of CP4D cluster but which CP4D is able to natively connect to.

I've used the DVD Rental Database described in section [Sample data assets](/Setup%20WKC%20demo%20environment/Data%20Assets/Sample_data_assets.md). The database has been deployed twice on different schemas to serve as the source and as a target for the pipelines lineage.

On the database which is a planned Target 2 assets have to be created on top of pre-built content. Use any available method to connect to database and run the following SQL statements inside the target schema.

1. Table for Target connection of ETL. You may need to change schema name "public" to a different value.

```
CREATE TABLE public.full_address (
	address_id int4 NOT NULL,
	address varchar(50) NOT NULL,
	address2 varchar(50) NULL,
	district varchar(20) NOT NULL,
	postal_code varchar(10) NULL,
	city varchar(50) NOT NULL,
	country varchar(50) NOT NULL,
	full_address varchar(300) NOT NULL
);
```

2. View for data summarization. That will extend the lineage flow diagram at the demo time.

```
CREATE OR REPLACE VIEW public.customer_and_address
AS SELECT cu.customer_id AS id,
    (cu.first_name::text || ' '::text) || cu.last_name::text AS name,
    a.full_address
   FROM my_customer cu
     JOIN full_address a ON cu.address_id = a.address_id;
```

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

For Platform access select "Assign roles directly" and click Next

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

Name the connection "PostgreSQL_lineage_target" and validate the connection works

![alt text](/Data%20Lineage/images/pl_conn-8.png)

Done with Target database connection creation. You should now see 2 available connections for PostgreSQL in the list of Platform connections

![alt text](/Data%20Lineage/images/pl_conn-9.png)

## 2.4 Creation of the Projects and Catalogs

For the lab purposes you would need to have one Catalog and at least one Project.

> [!WARNING]
> When working on the same environments, depending on the setup it may lead that several users working with the same databases but with different MDI procedures may result in getting in Manta repository duplicated entries which you will have later to resolve. Therefore it is strongly recommended that for this specific lab you would utilise your own database copies as separate Platfrom connections and your own DataStage ETL jobs named uniquely in your own Project. Advice for the multi-user lab facilitation is that all the assets created should be suffixed with underscore and user initials, e.g. \_XX

### Create your Project

Although the user you have created earlier has the permissions to create Projects, on this step we still will utilise the Admin user for this task