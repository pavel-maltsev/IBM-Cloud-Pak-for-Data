# Objectives

With this lab exercise section you will prepare additional assets for the lineage. This exercise should be performed under your newly created user account, not admin.

> [!IMPORTANT]
> Before you start this lab section, make sure you complete Step 0 - Pre-work scenario.
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

## 3.2 Preform Metadata Import for the data assets

You now need to use the connections to import specific data assets required to build ETL job.
