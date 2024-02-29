# Objectives

This chapter of the lab exercise will guide you through the metadata import procedure for scanning the Data sources and DataStage ETL pipelines with Manta lineage scanners built in the IBM Cloud Pak for Data platform.

> [!IMPORTANT]
> Before you start this lab section, make sure you complete [Step 1 - Data Pipelines creation scenario](/Data%20Lineage/Lab1_data_lineage_ETL_Postgres_1_Data_Pipelines_creation.md).

> [!TIP]
> The import of metadata can be performed just with few clicks I describe below. Few notes to be taken are that you should have to import:
>
> 1. You should import the full set of assets mentioned in this demo script, paying attention to the parameters selected.
> 2. You may see missing connections in the lineage result if you perform the import in wrong order. If this happens, re-run the created MDI lineage jobs in proper order. This should fix the results.

# 5. Lineage metadata import

At the moment you should have already created the connections to the PostgreSQL source and target databased in your CP4D Project. We will reuse those in this exercise.

## 5.1 Metadata import of Sources and Targets

First step of getting the proper lineage result is to scan the source and target databases for the data storage objects and internal pipelines. In this case those are tables vs PostgreSQL functinon and views. Even when having no external data transformation pipelines those internal PostgreSQL pipelines may bring the value in understanding the data flows inside your application repositories and support your data privacy and GDPR initiatives.

Open your project, switch to the Assets tab and click the "New asset" button.

![alt text](/Data%20Lineage/images/mdi_l_db-0.png)

Select the "Metadata import" tile from Data access tools section.

![alt text](/Data%20Lineage/images/mdi_l_db-1.png)

On the next screen valiate that you have enough of the Available scripts (for this lab exercise you should have at least 200), then select "Get lineage" tile.

![alt text](/Data%20Lineage/images/mdi_l_db-2.png)

Click Next button

![alt text](/Data%20Lineage/images/mdi_l_db-3.png)

When asked to Define the details, provide mdi_l_PosrgreSQL as the name of this metadata import.

![alt text](/Data%20Lineage/images/mdi_l_db-4.png)

Click Next again

![alt text](/Data%20Lineage/images/mdi_l_db-3.png)

You can't store the lineage results in the Project, you can only place those in one of availalbe Catalogs instead. That's why on the screen with selection of the target you will have the Project tile greyed out.

With Catalog tile checked in the list below select the "Lineage Demo Catalog" as the catalog that you want to import the metadata into. In your real project you may have a different catalog where all the lineage results are being collected, but so far we use for the lab the separate one we have created.

![alt text](/Data%20Lineage/images/mdi_l_db-5.png)

Click Next again

![alt text](/Data%20Lineage/images/mdi_l_db-3.png)

Now you have to select the scope of the metadata import scan.

![alt text](/Data%20Lineage/images/mdi_l_db-6.png)

Since the lineage scan tries to analyse the all possible relations and pipelines in the database, the lowest level of the assets to scan would be the schema inside your Source or Target PostgreSQL connections. Select firstly the schema with PostgreSQL_lineage_source connection (public in my case) and then do the same for the PostgreSQL_lineage_target schema connection.

![alt text](/Data%20Lineage/images/mdi_l_db-7.png)

Press Select when both schemas appear in the right column of "selected assets"

![alt text](/Data%20Lineage/images/mdi_l_db-8.png)

Check the metadata import scope on next page and then click Next button

![alt text](/Data%20Lineage/images/mdi_l_db-9.png)

You don't need to schedule the mdi runs in this lab. Click Next button.

![alt text](/Data%20Lineage/images/mdi_l_db-3.png)

On the Advanced options page make sure all 3 checkboxed are selected and click next again

![alt text](/Data%20Lineage/images/mdi_l_db-10.png)

Click Create on the next screen

![alt text](/Data%20Lineage/images/mdi_l_db-11.png)

You should get the following messge on the start of the mdi job:

![alt text](/Data%20Lineage/images/mdi_l_db-12.png)

After the completion you should receive the list of the assets you have just scanned into the "Lineage Demo Catalog"

![alt text](/Data%20Lineage/images/mdi_l_db-13.png)

Review the scan results in case of Warnings or Error messages.

The log messages in the log will lead you to origin of those errors. Fix those together with your DB and CP4D administrators.

![alt text](/Data%20Lineage/images/mdi_l_db-14.png)

Re-run the mdi scan again when issue origin is fixed.

> [!TIP]
> You may also import the Source and Target schemas separately in their own MDI jobs in case if you have any issues with test environment performance or large number of assets in a real project.

Now you have your Source and Target metadata successfully imported into the CP4D catalog and available for other Data Stewards.

## 5.2 Metadata import of DataStage ETL pipelines

First step of getting the proper lineage result is to scan the data pipelines. In this lab we have created 2 ETL jobs in the CP4D project that move the data between 2 databases we've scanned on the previous step. Now it's time to import this ETL lineage information in to our Catalog.

Open your project, switch to the Assets tab and click the "New asset" button.

![alt text](/Data%20Lineage/images/mdi_l_db-0.png)

Select the "Metadata import" tile from Data access tools section.

![alt text](/Data%20Lineage/images/mdi_l_db-1.png)

On the next screen select "Get ETL job lineage" under the "Get lineage" section

![alt text](/Data%20Lineage/images/mdi_l_dsx-0.png)

Click Next button

![alt text](/Data%20Lineage/images/mdi_l_dsx-1.png)

On the Define details screen provide the mdi_l_dsx_Migrate as the name of this metadata import.

![alt text](/Data%20Lineage/images/mdi_l_dsx-2.png)

Click Next button

![alt text](/Data%20Lineage/images/mdi_l_dsx-1.png)

Same as in the previous scenario you would have only option to import in the Catalog, but not the Project. Select the "Lineage Demo Catalog" and press Next

![alt text](/Data%20Lineage/images/mdi_l_dsx-3.png)

The DataStage project on CP4D is a common container for ETL activites of specific business area. It can be treated as the schema inside the database. Therefore the screen to select the scope would be different from what you've seen on the database metadata import steps. This time we will use the checkbox "Select all DataStage flows and their dependencies in the project...". This will automatically pick all the relevant ETL meta definitions in the single metadata import job. If you want to focus on the selected import, you could proceed with the "Select scope" button and manually pick the flows and their sub-parts you are interested in. When doing manual selection, the minimum to be choosen is the set of connections the DataStage uses and the Flow itself. This will relate the job to the proper sources and targets it uses to complete the lineage picture across all technical metaassets.

Click Next button 3 times. We would skip schedulling and setting additional options. Then click Create button.

![alt text](/Data%20Lineage/images/mdi_l_dsx-4.png)

MDI for DataStage flows in the project will start immediately.

![alt text](/Data%20Lineage/images/mdi_l_dsx-5.png)

When job complete you will see the full list of the objects imported

![alt text](/Data%20Lineage/images/mdi_l_dsx-6.png)

If you want to browse the lineage details, you can jump to the Catalog from this screen by clicking the lineage target link on the right side

![alt text](/Data%20Lineage/images/mdi_l_dsx-7.png)

This concludes the Datastage ETL pipelines metadata import scenario and this lab exercise chapter. [The "Review Lineage Results" script is located by this link](/Data%20Lineage/Lab1_data_lineage_ETL_Postgres_3_Lineage_review.md)

[Back to Data lineage HOL page](/Data%20Lineage/Data_Lineage_lab_exercise.md)
