# Introduction

This exercise demonstrates additional capabiliites on assessment data quality of the governed data assets. Clients in most of the cases ask for the way to automate the DQ assessment of the data assets with minimal manual efforts.

This exercise extends the Metadata Enrichment lab to automatically add new DQ validations on the data assets after they are run through discovery, profiling and term assignment procedures.

As the final results, the DQ remediation task will also be created which can be assigned to data stewards for resolution within workflow logic.

# Creation of the SLA rule for generic control on completeness of the data asset

From the main CP4D navigation menu switch to the Rules list. It may contain the previously imported Data Governance Rules if you've run the [step 2 - Rules section lab](/Setup%20WKC%20demo%20environment/Business%20Terms/Business_terms_upload.md). In my case the screen shows 2 Data Governance rules.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla.png)

In top-right select 'Add rule - New data quality SLA rule'

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-1.png)

Fill in the Name and Business definition as shown on screenshot

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-2.png)

Press 'Next'

On the next screen in the drop-down of 'Any data asset' select 'with one of the business terms'
![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-3.png)

> [!IMPORTANT] > **This term and all the following** in the process should be selected from the 'Customer 360 Degree View' category! That is important since it should be consistent with other parts of the demo scenario. If not, you will not see the proper results at the end!

In the field to the right from it start typing 'cust' and find in the drop-down list the 'Customer' business term. Primary category of the term is listed right below the term itself on the selection screen, but is not visible after you confirm selection.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-4.png)

Press 'Add quality criteria' button

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-5.png)

Select 'completeness dimension score' equal to or greater than '99.5'

Then add another quality criteria and select 'overall data quality score' equal to or greater than '99'

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-6.png)

Scroll down and press 'Select +' button in the 'Remediation task' section

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-7.png)

Select radiobutton next to 'default' workflow and then press 'Select'§ button in the bottom-right corner

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-8.png)

Press now the Create button at the bottom-right corner to confirm the creation of the SLA rule.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-9.png)

You will get confirmation on the SLA rule created successfully

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-10.png)

Return back to the Rules top level screen

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-11.png)

# Creation of the SLA rule for DQ control on specific attributes of the selected assets

Repeat the actions of the first SLA rule creation to setup the second DQ control.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-12.png)

Press 'Next'

Create the following rule with terms 'Customer' and 'Customer Account' from the 'Customer 360 Degree View' category. Set the DQ criteria 'overall data quality score' to '99'.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-13.png)

Next click 'Add subcondition' button below

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-14.png)

Select the business terms from the same category in the section of the 'any column with': 'Email address', 'Telephone Number', 'Address', 'Work Address', 'Postal Address', 'Home Address', 'First name', 'Last name'.

Select 'accuracy dimension score' equal to or greater than '99'.
![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-15.png)

Same as on previous SLA rule, select the 'default' workflow for remediation. Create the SLA rule.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-16.png)

# Validation of the SLA rules

In order to validate the SLA rules you should complete firstly the Metadata Enrichment part of the demo. You are expected to have completed all steps there and own Data Quality project with 'mde_db2_on_cloud_my_schema' MDE asset present (or the one you've created and run on your own database and data assets)

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-17.png)

First you have to make sure that SLA rules are enabled on the level of the project. Otherwise you would see the following message in DataQuality tab of the asset

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-18.png)

Either on this screen press the button 'Enable SLA rules'

Or move to the 'Data Quality' project, 'Manage' tab.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-19.png)

Switch to the Metadata Enrichment screen under Tools from the menu on the left and scroll to the SLA rules section. Activate monitoring of SLA rules by the switch there.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-20.png)

On the BANK_CUSTOMER asset View Data quality details screen

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-21.png)

the SLA setting warning will now disappear.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-22.png)

Please check that all the steps in MDE demo have been complete and you will see the business terms assigned to the data assets:

- BANK_ACCOUNTS - Customer Account
- BANK_CLIENT - Customer
- BANK_CUSTOMERS - Customer

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-23.png)

The SLA rules won't be applied automatically to the previously enriched assets. You should either select few of the required assets and on the selected assets blue line click on the 'Enrich' button, or rerun the full scope MDE by selecting 3 dots on top line and 'Enrich all assets'

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-24.png)

You will get notifications on the MDE job start

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-25.png)

You may check the status of the job on the 'Data Quality' project page, tab 'Jobs', then selecting the 'mde_db2_on_cloud_my_schema' job im my case.

There are the execution statuses of the previous run and current run listed. Wait until the current job completes.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-26.png)

You will get notification that MDE job has been complete even if not on that screen

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-27.png)

Return to the Data Quality project Assets tab and click on the BANK_CUSTOMERS table (data asset)

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-28.png)

On the Data Quality tab of the asset you will see the SLA compliance rules assessment results. In my case both SLA rules I've created in this demo have been violated.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-29.png)

Click on the Customer Data completeness SLA rules and inspect the details. You can see the Expected and Current scores for 'Completeness' and 'Overall data quality'. That is the reason!

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-30.png)

Next check the results for the BANK_CLIENTS data asset

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-31.png)

On the Data Quality tab you will see that there are no DQ SLA violations. But you can still investigate those.
![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-32.png)

And if you switch to the Rule details tab you will see the Rule definition as you've created it.

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-33.png)

> [!TIP]
> If you need to fas modify rule in this lab, you may use the 'Edit SLA rule' button i the top-right corner.

Also you'd need to check the BANK_ACCOUNTS data asset for SLA assessment results.

Can you explain why on the Data Quality tab you see only one SLA rule while on previous data assets there have been two?

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-34.png)

At the same time together with acqnowledgement of the SLA violations on the data assets page we have received the Tasks for remediation in the Task inbox accessible from main CP4D navigation menu

![alt text](/Setup%20WKC%20demo%20environment/SLA%20Rules/images/sla-35.png)

This concludes the SLA rules lab section.

[Back to Setup IKC demo environment page](/Setup%20WKC%20demo%20environment/WKC_demo_setup_general_steps.md)
