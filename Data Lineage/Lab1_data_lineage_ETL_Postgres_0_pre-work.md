# 1. Objectives

The goal of this excersise is to demonstrate the capabilities of IBM Cloud Pak for Data on monitoring the data lineage, discovery of the internal pipelines in databases and external to then pipelines of DataStage ETL jobs running on IBM CP4D by using the IBM Manta software service.

The lab will use 2 databases and 2 ETL jobs to demonstrate the value of data lineage with Manta.

# 2. Pre-work

## 2.1 Data sources

Before starting the lab excersise please prepare 2 RDBMS data sources. Here we are to use the Postgres available databases deployed outside of CP4D cluster but to which CP4D cluster is able natively connect to.

> [!NOTE]
> If you are using thess lab instructions as a part of the guided enablement, you should be given the credentials for PostgreSQL databases by your instructor.
>
> If you do this lab on your own outside of IBM facilitated enablement, please check the link below on where to find the database dump and how to deploy that on your personal environment.
>
> Feel free to contact me directly if support with content is needed.

I've used the DVD Rental Database described in section [Sample data assets](/Setup%20WKC%20demo%20environment/Data%20Assets/Sample_data_assets.md). The database has been deployed twice on different schemas to imitate separate source and target for the pipelines lineage.

On the database which is a planned as a Target, 2 assets have to be created on top of pre-built content. Use any available method to connect to database and run the following SQL statements inside the target schema.

1. 2 tables for Target connection of ETL. You may need to change schema name "public" to a different value which exists in your database.

```sql
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

```sql
CREATE TABLE public.my_customer (
	customer_id int4 NOT NULL,
	store_id int2 NOT NULL,
	first_name varchar(45) NOT NULL,
	last_name varchar(45) NOT NULL,
	email varchar(50) NULL,
	address_id int2 NOT NULL,
	activebool bpchar(1) NOT NULL,
	create_date date NOT NULL,
	last_update timestamp(6) NULL,
	active int4 NULL
);
```

2. View for data summarization. That will extend the lineage flow diagram at the demo time.

```sql
CREATE OR REPLACE VIEW public.customer_and_address
AS SELECT cu.customer_id AS id,
    (cu.first_name::text || ' '::text) || cu.last_name::text AS name,
    a.full_address
   FROM my_customer cu
     JOIN full_address a ON cu.address_id = a.address_id;
```

> [!IMPORTANT]
> Make sure that the PostgreSQL account under which you plan to work with these 2 databases has the all necessary privileges granted to select and modify the table data, truncate and re-create tables, request metadata details, including for the newly created by the scripts above ones.

## 2.2 Services used in the lab

For successful lab execution CP4D instance should have IKC, Manta and DataStage Ent. services deployed.

Manta license used on the environment should have about 100 of the "Unused scripts" available for the lab.

> [!TIP]
> Generic information about the cluster and services deployment can be found [here](/Setup%20WKC%20demo%20environment/Pre-work.md).
>
> Use this [link to validate or update your Manta license](/Data%20Lineage/Licensing.md).

## 2.3 Environment setup

### User creation

We would separate this demo scenario from the other topics and also demonstrate how the Connectors can be freely shared by the authorised user with other Data Stewards.

For that we would need the different user to be created in CP4D main menu - Administration - Access Control.

![alt text](/Data%20Lineage/images/user_create-0.png)

Click "Add users" in the right-top corner.

![alt text](/Data%20Lineage/images/user_create-1.png)

I will create John Lineage here.

![alt text](/Data%20Lineage/images/user_create-2.png)

For Platform access select "Assign roles directly" and click Next.

![alt text](/Data%20Lineage/images/user_create-3.png)

![alt text](/Data%20Lineage/images/user_create-4.png)

Provide the user Role of Data Steward and click Next again.

![alt text](/Data%20Lineage/images/user_create-5.png)

![alt text](/Data%20Lineage/images/user_create-4.png)

Browse the summary and click Add.

![alt text](/Data%20Lineage/images/user_create-6.png)

You will have now at least 2 users avaialble for this demo.

![alt text](/Data%20Lineage/images/user_create-7.png)

### Create Platform Connections

In this chapter the Platform connections are to be created for the source and target Postgres databases.

Follow the CP4D main menu - Data - Platform connections menu.

![alt text](/Data%20Lineage/images/pl_conn-0.png)

On the right side click "New connection" button

![alt text](/Data%20Lineage/images/pl_conn-1.png)

The revealed screen form shows you the variety of connections available on the CP4D platform. Those can be created in a centralised way and then re-used in several activities. The top-right link to "Supported connection types" will provide you documentation on connectivity options and applicability of those for specific functional areas.

Use the "Find connectors" line on top to search for "Postgre".

![alt text](/Data%20Lineage/images/pl_conn-2.png)

Click the one named PostgreSQL and then click Select.

![alt text](/Data%20Lineage/images/pl_conn-3.png)

Provide the Name "PostgreSQL_lineage_source" and other parameters for the database you've created on the earlier steps of the pre-work.

When entering the Credentials data choose the radio button for "Credential setting" as "Shared".

![alt text](/Data%20Lineage/images/pl_conn-4.png)

When done, press "Test connection" on the top-right of the page.

![alt text](/Data%20Lineage/images/pl_conn-5.png)

You should receive confirmation on successful results.

![alt text](/Data%20Lineage/images/pl_conn-6.png)

Click "Create" and the bottom-right.

![alt text](/Data%20Lineage/images/pl_conn-7.png)

Done with Source database connection for Lineage lab. Next please repeate the same steps to create the connection for the Target database.

Name the connection "PostgreSQL_lineage_target" and validate the connection works.

![alt text](/Data%20Lineage/images/pl_conn-8.png)

Done with Target database connection creation. You should now see 2 available connections for PostgreSQL in the list of Platform connections:

![alt text](/Data%20Lineage/images/pl_conn-9.png)

## 2.4 Creation of the Projects and Catalogs

For the lab purposes you would need to have one Catalog and at least one Project.

> [!WARNING]
> When working on the same environments, depending on the setup it may lead that several users working with the same databases but with different MDI procedures may result in getting in Manta repository duplicated entries which you will have later to resolve. Therefore it is strongly recommended that for this specific lab you would utilise your own database copies as separate Platfrom connections and your own DataStage ETL jobs named uniquely in your own Project. Advice for the multi-user lab facilitation is that all the assets created should be suffixed with underscore and user initials, e.g. \_XX

### Create your Catalog

As the common practice, the Data Catalogs are defined on the level of the CDO office and not by specific Data Stewards. Therefore, using user Admin select CP4D main menu - Catalogs - All Catalogs.

![alt text](/Data%20Lineage/images/containter_creation-0.png)

Use the "New catalog" button on the top-right side.

![alt text](/Data%20Lineage/images/containter_creation-1.png)

Provide the name "Lineage Demo Catalog". There is no need to chagne other parameters this time.

![alt text](/Data%20Lineage/images/containter_creation-2.png)

Click "Create" at the bottom-right corner of the screen.

![alt text](/Data%20Lineage/images/containter_creation-3.png)

On the screen of the newly created catalog select "Access Control" tab, where you will see that only Admin user is currently listed as the one administering it.

![alt text](/Data%20Lineage/images/containter_creation-4.png)

Click "Add collaborators" then "Add user" button.

![alt text](/Data%20Lineage/images/containter_creation-5.png)

In the pop-up window select Role: Adming and start typing "lineage" on the line below until your user will appear on the screen:

![alt text](/Data%20Lineage/images/containter_creation-6.png)

Click "Add". You will now see 2 users in you catalog, both with Admin Role.

![alt text](/Data%20Lineage/images/containter_creation-7.png)

Click the top-right corner button and Log out of the system, then log in as your user (John Lineage in this demo).

> [!CAUTION]
> All the rest steps should now be done under your personal user account.

### Create your Project

Open CP4D main menu - Projects - All projects.

![alt text](/Data%20Lineage/images/containter_creation-8.png)

Click "New project" button in the top-right corner.

![alt text](/Data%20Lineage/images/containter_creation-9.png)

Select the first option "Create an empty project". On the next screen provide the "Lineage Project" as the name.

![alt text](/Data%20Lineage/images/containter_creation-10.png)

Click "Create" button.

![alt text](/Data%20Lineage/images/containter_creation-11.png)

When project in place, Click "Manage" tab and then sub-tab of "Access Control". You will see your user as the only one in the list.

Next step is to add Administrator as a collaborator to the project. This activity is not mandatory, but if the lab is done on the shared environment with several users, then Administrator would be able to support you on the journey. Same way in regular system usage you would add more colleagues from your organization to team up on specific task with you.

![alt text](/Data%20Lineage/images/containter_creation-12.png)

Select "Add collaborators - Add users" button. In the new window start typing "admin". For the Admin user found in Search results select the Role as either Admin or Editor.

![alt text](/Data%20Lineage/images/containter_creation-13.png)

Click "Next", now you can see both users in the list of collaborators on this project with their roles defined.

## 2.5 IKC to Manta services integration

When preparing to run this lab chapter, make sure your CP4D environment is setup properly. There is a must have condition that services of IKC and Manta are setup properly and both are integrated. This can be checked in your Project when on the Asset page selecting "New asset" button and then clicking the tile Metadata import. If you immediately then jump into the screen where you can enter the metadata import name, then your IKC service doesn't have a connection to Manta service setup. Instead, you should see the full set of the tiles related to the various metadata import options together with the number of available licensed Manta scripts to utilise, as per screenshot below.

![alt text](/Data%20Lineage/images/mdi_tiles.png)

If you don't see such a screen ask you RedHat Openshift administrator to restart full set of pods which have either Manta or WKS in the name. This should help if the environment has been once setup correctly. From the moment the pods have been restarted and obtained "running" status, you should be able to utilise those MDI capabilities in full scale.

Without this setup step resolved you won't be able to complete the lab!

This concludes the Step 0 - Pre-work for the Data lineage lab.

You may now proceed with [Step 1 - Data Pipelines creation scenario](/Data%20Lineage/Lab1_data_lineage_ETL_Postgres_1_Data_Pipelines_creation.md).
