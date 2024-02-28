# Objectives

This chapter of the lab exercise will provide the sample flow, how you may discover your metadata lineage results and demonstrate that to business and technical teams.

> [!IMPORTANT]
> The lineage results are only available if you cover all the previous steps of the lab, including the last one [Step 2 - Lineage metadata import](/Data%20Lineage/Lab1_data_lineage_ETL_Postgres_2_Lineage_metadata_import.md).

# 6. Review of lineage analysis

So far the whole goal of the excercise was to create the set of metadata definitions to imitate couple of corporate applications with repository and the data pipelines moving data between those.

We have created:

- Source database PostgreSQL dvdrental
- Target database PostgreSQL public
- DataStage ETL job dsx_Migrate_Customers
- DataStage ETL job dsx_Migrate_Full_Address

Next we have used the IKC metadata import processes to retrieve the metadata from those and publish that to the catalog "Lineage Demo Catalog"

The import has been enabled by Manta technology natively integrated into IBM Cloud Pak for Data v4.8.1 as the containerised service deployed next to IBM Knowledge Catalog. Due to this we can have several levels of representations of imported metadata. One is the high level business lineage available for the users of catalog. This one is sufficient for understanding the data pipelines, data sources and targets, dependencies between those. It will help business users in many cases, e.g. answering questions on:

- how the data is managed
- where it is located
- which systems interract within specific business initiative or a series of pipelines
- audit for variety of GDPR questions, etc

The high level of the details covers most of the questions from business departments. The deep dive technical lineage can provide another perspective for the same information with much more technical details:

- what are specific scripts and code used inside data sources pipelines
- how did lineage change through the time
- which metadata object have been brought to repository recently and which have been removed
- high level and very atomic (field level) details and impact analysis

## 6.1 CP4D Catalog review

Open the All Catalogs from CP4D main menu and select "Lineage Demo Catalog" to work with.

![alt text](/Data%20Lineage/images/lineage_review-0.png)

Now you see all metadata objects imported during this exercise. Open Filter icon in the search line between the Recently added section and the full list of the catalogued objects. Select in Asset type "Data integration job". In the search line start typing "dsx\_". The list will change and show you only 2 filtered items.

![alt text](/Data%20Lineage/images/lineage_review-1.png)

Click on the "dsx_Migrate_Customers..." asset. The screen with show up this entity. The DataStage flow contatins 2 nested assets which are the connectors "customer_Target" and "customer_Source". Those are the names we've used when designed the 2 stages Datastage flow.

![alt text](/Data%20Lineage/images/lineage_review-2.png)

![alt text](/Data%20Lineage/images/lineage_review-3.png)

You may investigate the asset details further, e.g. by clicking on the "customer_Source" connector and see the columns on the links of the job which have also been imported to the Catalog as separate named objects.

Return back on the primary catalog page and now select the DataStage flow "dsx_Migrate_Full_Address...". That one has more complex logic and we would start from browsing it's details.

![alt text](/Data%20Lineage/images/lineage_review-4.png)

On Overview tab you can find all relevant technical details on the flow composition, including the full set of transormation stages we've used (Join, Merge, Transformer).

When you proceed with the Data Governance initiative, the descriptions to this governance articacts may be added, like related business terms, classifications. Also users may leave the reviews on a separate tab and rate this asset.

On the Asset tab the detailed information is presented in a form of nested tree.

![alt text](/Data%20Lineage/images/lineage_review-5.png)

When you click on the Lineage tab the high-level data flows would be revealed.

![alt text](/Data%20Lineage/images/lineage_review-6.png)

If you would need to extract this information to external documentation or publish on some corporate project portal, you may use the buttons on the top-right part of the screen to extract the data from Catalog in few ways

![alt text](/Data%20Lineage/images/lineage_review-7.png)

2 options are available for the export: "Tabular view in CSV file" and "Image of lineage graph in PDF".

![alt text](/Data%20Lineage/images/lineage_review-8.png)

You may test those by selecting the radio button and clicking "Export image" button.

On the graph you may recognise that the connecting lines between sources for the flow and target are in a form of dotted, not solid lines. Also it may look strange that the data sources are not named the same way as we've used in the DataStage flow designer.

By default, Business lineage graph hides the details of the larger scale data pipelines into a packed form. To reveal the detailed picture click on the 3 dots right to the Datastage asset name and select "Show all" in a drop down menu.

![alt text](/Data%20Lineage/images/lineage_review-9.png)

This will show you the direct source and target tables of the DataStage flow which we can recognise from the past. Nevertheless, the information about where from the data is landed to e.g. address or country tables is still present on the screen which gives the business users more holistic understanding of the data pipelines within this and neighbouring assets.

![alt text](/Data%20Lineage/images/lineage_review-10.png)

Additional subset of information can be displayed if the "Expand" button is clicked next to informational message saying that datastage flow consists of 7 internal stages.

![alt text](/Data%20Lineage/images/lineage_review-11.png)

Also in the bottom-right corner you will find the legend and map of the lineage diagram.

![alt text](/Data%20Lineage/images/lineage_review-12.png)

If we would like to discover the lineage in more details, including transformation scripts or changes through the time we should then switch to the technical lineage screen. Open the first Overview tab of the same job.

On the right side of the screen there should be the detailed information pane "About this asset". If not there, click the ![alt text](/Data%20Lineage/images/lineage_review-13.png) sign in top-right corner.

![alt text](/Data%20Lineage/images/lineage_review-14.png)

Use the "Go to asset's technical data lineage" link to drill down. If you click the link directly, technical lineage will be opened in the same window. If you want to compare the 2 representations, you may like to keep both windows, so click on the box with arrow sign left from the link name ![alt text](/Data%20Lineage/images/lineage_review-15.png)

> [!TIP]
> Another option can be to use the "Go to asset source" link right below this one. This will bring you directly to the DataStage flow design canvas in the Project where flow resides, if you have corresponding privileges setup for your user.

## 6.2 Technical lineage representation of the same pipeline

Clicking on the technical lineage transfers you to the next part of demonstration - technical lineage

![alt text](/Data%20Lineage/images/lineage_review-16.png)

Use the Options menu on the top-right corner, switch to the "Detail" tab and use the radio buttons in column "H" to switch representation to the High level of details which will display all the columns for current subset of visualised assets. Click Apply.

![alt text](/Data%20Lineage/images/lineage_review-17.png)

if you click on the full_address column in full_address table of public schema, the graph will show you the complete set of the attributes and transformations that supply the data for this column, same as consumers of this column data later on. Keep in mind that representation of the information at the moment is limited by selection of the lineage scope we've used when switched to this screen from IKC Catalog. All the relations are valid, but potentially this could be not the complete list.

To show the difference, let us show the full set of the pipelines feeding the final "customer_and_address" view we've created on the pre-work step for this lab.

Right-click the "customer_and_address" view name and select "Restart visualization from this element" in the drop down menu.

![alt text](/Data%20Lineage/images/lineage_review-18.png)

You will now see that this view is originating from 2 tables: "my_customer" and "full_address". Those are populated with data by 2 ETL jobs: dsx_Migrate_Customer and dsx_Migrate_Full_Address

If you click on the "name" column in the "Customer_and_address_BODY" script and also hover the "TT" ![alt text](/Data%20Lineage/images/lineage_review-20.png) icon, you will see the data flow, including the formula hidden in the view DDL script itself.

![alt text](/Data%20Lineage/images/lineage_review-19.png)

## 6.3 Lab exercise results technical lineage

Click on the "IBM Automatic Data Lineage" logo on the top-left corner.

![alt text](/Data%20Lineage/images/lineage_review-21.png)

This will close the current screen and move you to the Repository browse screen. Repository contains the full detailed result of all the scans on all the object performed so far.

![alt text](/Data%20Lineage/images/lineage_review-22.png)

In our lab we have worked with technologies of IBM DataStage and PostgreSQL. Those are now visible in the list.

If you open the DataStage section you will see both flows we've imported with MDI job in IKC Project.

![alt text](/Data%20Lineage/images/lineage_review-23.png)

You may click on the dsx_Migrate_Full_Address flow to explore the details.

![alt text](/Data%20Lineage/images/lineage_review-24.png)

Next drag and drop the 2 dsx*Migrate*... flow names (located on the level right below the "Lineage Project") to the section of Selected for lineage on the right-botton side of the screen. As alternative, you could use the "+" sign next to those lines.

![alt text](/Data%20Lineage/images/lineage_review-25.png)

In the Visualization parameters select "Custom settings".

![alt text](/Data%20Lineage/images/lineage_review-26.png)

Right to selected line choose the proper options for visualization as per screenshot:

![alt text](/Data%20Lineage/images/lineage_review-27.png)

Click Visualize button.

![alt text](/Data%20Lineage/images/lineage_review-28.png)

This will display the complete lineage of our lab exercise: 2 created Datastage flows, all the PostgreSQL source and target tables, custom built view in PostgreSQL target database.

![alt text](/Data%20Lineage/images/lineage_review-29.png)

# 6.4 Additional technical lineage content available

If you recognized that during pre-work, the source/target databases you've restored from backup had much more content rather than the tables we've used for this lab. Lets explore some more lineage analysis result available after the MDI scans we've performed.

Right-click on the "address" table on the source schema of dvdrental and select "Restart visualization for this element".

![alt text](/Data%20Lineage/images/lineage_review-30.png)

The diagram rebuilt shows the additional data targets which consume data from "address" table.

![alt text](/Data%20Lineage/images/lineage_review-31.png)

Select the column address in this table. Use the options menu in the top-right corner to select High level of details and click Apply.

![alt text](/Data%20Lineage/images/lineage_review-32.png)

This will show you that column "address" is also used in 2 other views "customer_list" and "stall_list".

![alt text](/Data%20Lineage/images/lineage_review-33.png)

This provides the information of potential impact of "address" reference table and specifically "address" column metastructure changes to other use cases, not only to DataStage flow which we've been aware before. E.g. change of the column length or type should be agreed with all interested parties on neighbouring projects, not only on ETL.

Click on the "IBM Automatic Data Lineage" logo on the top-left corner.

![alt text](/Data%20Lineage/images/lineage_review-21.png)

On the top-right of the screen remove the previously selected content for lineage by pressing trash bin icons next to the dsx asset names

![alt text](/Data%20Lineage/images/lineage_review-34.png)

Now drag and drop to that section schema named "public" in the "dvdrental" database

![alt text](/Data%20Lineage/images/lineage_review-35.png)

![alt text](/Data%20Lineage/images/lineage_review-36.png)

Click Visualize button with Custom settings selected same way as last time.

> [!CAUTION]
> We can do that for the whole schema as soon as the content of this database is not extensive. In real live you would select smaller atomic groups of objects for analysis as per your real investigation task. In case if selected set of objects contains too many assets, you will receive the warning message with corresponding content while initiating visualization.

Feel free to play around with filters, details and dependency analysis on the representation screen displayed

![alt text](/Data%20Lineage/images/lineage_review-37.png)

This concludes the [HOL for Data lineage](/Data%20Lineage/Data_Lineage_lab_exercise.md).
