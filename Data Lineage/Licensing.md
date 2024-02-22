# IBM Infosphere Knowledge Catalog - Data lineage

## 1. License Metrics

IBM IKC data lineage is enabled by the Manta service which is licensed on the CP4D platform by the specific terms, called Scripts.

Support portal of IBM provides the following information about right process to count those scripts:

MANTA Automated Data Lineage for IBM Cloud Pak for Data ("the Program") is sold in Resources Units (RU), where one RU is 30,000 Scripts. A Script is a unique code element defined by the Product implementation, taking into account specifics of each particular technology. This article describes the details of currently supported scripts and how to determine what counts as a Script.

[The details can be found by this link](https://www.ibm.com/support/pages/manta-automated-data-lineage-ibm-cloud-pak-data-script-counting-details)
This document contains details on how to count the scripts for each of the supported data sources or data transformation solutions.
The version as per Jan 2024 is saved as PDF [here](https://github.com/pavel-maltsev/IBM-Cloud-Pak-for-Data/blob/main/DataLineage/docs/MANTA_Automated_Data_Lineage_for_IBM_Cloud_Pak_for_Data_Script_counting_details.pdf), but please ensure you use the latest information from the vendor available online or provided by sales representatives.

## 2. License deployment

Before start using Manta add-on for IKC the license key file should be added. This provides to Manta information on script number purchased and left after scans done so far.

To deploy the license file the following steps should be done:

1. Use the link which you have for CP4D main UI, e.g.

`https://xxxxx.cloud.techzone.ibm.com/zen/#/homepage`

2. Remove the section after the ibm.com and add there the url part for Manta Admin UI `cloud.techzone.ibm.com/manta-admin-gui/`

3. In the Manta Admin UI follow to the section "Configuration" and then to the tab "License"

![alt text](/Data%20Lineage/images/manta-license_1.png)

4. Check the "Maximum number of scripts" vs "Currently used scripts" values

5. Add the new license.key file if needed by pressing the "Upload new license" button

   ![alt text](/Data%20Lineage/images/manta-license_2.png)

You may use either drag and drop or menu selection for this operation on the next screen

![alt text](/Data%20Lineage/images/manta-license_3.png)

The updated number of scripts available should be now visible on the screen of Manta Admin GUI and also when perfroming MDI operation on the CP4D Project.
