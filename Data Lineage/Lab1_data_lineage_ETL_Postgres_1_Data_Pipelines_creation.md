# Objectives

With this lab exercise section you will prepare additional assets for the lineage. This exercise should be performed under your newly created user account, not admin.

> [!IMPORTANT]
> Before you start this lab section, make sure you complete [Step 0 - Pre-work scenario](/Data%20Lineage/Lab1_data_lineage_ETL_Postgres_0_pre-work.md).
>
> This exercise should be performed under your newly created user account, not admin.

Previously you have already deployed external PostgreSQL databases and created Platform connections to those in this CP4D invironment In order to perform further steps in this lab you would need to import the connections and the assets to your Project and create 2 Datastage flows which would represent data piplelines moving the data between repositories.

# 3. Connectivity and asset import

## 3.1 Import Platform connections

Login to CP4D under your John Lineage user and open the "Lineage Project" you have previously created.

On the Assets tab click the New asset button

![alt text](/Data%20Lineage/images/con-import-0.png)

![alt text](/Data%20Lineage/images/con-import-1.png)

In the "Data access tools" section select "Connection"

![alt text](/Data%20Lineage/images/con-import-2.png)

Use the "From platform" tab to browse available Platform connections.

In the list you should see connections for source and target PostgreSQL databases you have created earlier.

![alt text](/Data%20Lineage/images/con-import-3.png)

Select "PostgreSQL_lineage_source" connection and click "Select" button. You will be switched to the Connection Overview page where you can test the connector by pressing the "Test connection" button on the top-right corner

![alt text](/Data%20Lineage/images/con-import-4.png)

The green background status message confirms availability of the database and validity of the credentials.

![alt text](/Data%20Lineage/images/con-import-5.png)

Click "Create" button to import this connection to your Project.

![alt text](/Data%20Lineage/images/con-import-6.png)

Perform the steps above to also import "PostgreSQL_lineage_target" connection.

You should now see 2 connection assets in your project

![alt text](/Data%20Lineage/images/con-import-7.png)

## 3.2 Perform Metadata Import for the data assets

Next we use the connections to import specific data assets required to build ETL job.

The data assets can appear in the Project by several methods. One of the options is to bring the data files of supported format directly by drag&drop operation to the right side of the Project screen.

![alt text](/Data%20Lineage/images/mdi_data-0.png)

Alternative is to perform the Metadata Import job using the CP4D screenforms and the variety of connections available on the platform.

We will use this method to add the source and target tables for DataStage pipelines. As one of the DataStage flow designs we would have to work with 4 tables in single flow. Instead of building connections and providing each of the tables separately wihtin the Flow design, we can do the few clicks pre-work to create reusable data assets in the project and then refer to them in the ETL pipeline.

When in the "Lineage Project" screen and Asset tab, select the New Asset button on the top-right of the page.
![alt text](/Data%20Lineage/images/mdi_data-1.png)

Click Metadata Import tile in the Data access tools section

![alt text](/Data%20Lineage/images/mdi_data-2.png)

On the next page you see 2 rows with activites. First row is for the Metadata Discovery operation. It allows to import assets metadata into Catalog or Project. Second row provides capabilities to import the Lineage information from different source types.

This time use the first tile on the top row "Discover" which is called the same - "Discover".

![alt text](/Data%20Lineage/images/mdi_data-3.png)

When asked for Details, provide the "mdi_PostgreSQL_Source" as the name of the metadata import.

![alt text](/Data%20Lineage/images/mdi_data-4.png)

Click "Next"

![alt text](/Data%20Lineage/images/mdi_data-5.png)

When asked to select the target of import, choose "This project (Lineage Project)" as selected by default.

![alt text](/Data%20Lineage/images/mdi_data-6.png)

You will be prompted to select the scope. Use the Select connection button.

![alt text](/Data%20Lineage/images/mdi_data-7.png)

In the screen, use available PostgreSQL_lineage_source connection to find in your schema 4 tables we will use. Table names are:

- address
- city
- country
- customer

![alt text](/Data%20Lineage/images/mdi_data-8.png)

When corresponding checkboxes are selected, press Select button.

![alt text](/Data%20Lineage/images/mdi_data-9.png)

On the next screen you can validate the selected list and press Next button

![alt text](/Data%20Lineage/images/mdi_data-10.png)

![alt text](/Data%20Lineage/images/mdi_data-11.png)

The import should be processes immediately, so we won't schedule that now. Press Next button again.

On the Advanced options you have 3 checkboxes selected by default. No need to change those.

![alt text](/Data%20Lineage/images/mdi_data-12.png)

Click Next.

On the summary page review the MDI parameters and click Create.

![alt text](/Data%20Lineage/images/mdi_data-13.png)

Metadata import process starts with the confirmation message.

![alt text](/Data%20Lineage/images/mdi_data-14.png)

Wait until it's complete and the confirmation message in green will pop-up.

![alt text](/Data%20Lineage/images/mdi_data-15.png)

The PostgreSQL source database assets have been succesfully imported into the Project.

Now repeat the same metadata import operation for the assets of your PostgreSQL Target database.

Name your metadata import "mdi_PostgreSQL_Target" and import 2 named below tables from your target connection to the Project:

- my_customer
- full_address

![alt text](/Data%20Lineage/images/mdi_data-16.png)

Finish the Metadata import process to see 4 imported data assets in the Asset tab of the project.
