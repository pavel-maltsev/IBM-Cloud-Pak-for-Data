# Data protection rules basics

When the company creates the enterprise governed catalog of data it holds vast variety of the enterprise data of different flavours. At the same time, the idea to share the data from the sentralised location with the large amount of internal and external to company consumers puts under a risk the data security.

IBM data protection rules allow to flexibly and automatically protect the parts of the data assets from an authorised use by:

- setting up the rules of the game - what and how should be protected
- keeping in the catalog metainformation describing the data asset which is used then for proper classification of information and data protection
- automated application of the data protection to the data assets with distinguish between users who own the data vs users who only are entitled to reach some parts of this data.

# Pre-work

As a pre-work for this exercise you should have the second user with basic privileges to whom you will restrict access to data.

Create such a user. In the CP4D versions prior to 4.8.4 you could do that directly in the CP4D web based UI as administrator. In the v4.8.4. and after solution uses the Openshift authentication which means that the user should be firstly created in RH Openshift (either directly or inherited from LDAP) and then you can assign the roles of that user back in the CP4D web based UI.

Here we start this lab with assumption that user in OCP has been already created.

Login to the platform with your extended privileges which allow modify users and work on acccess control. From main CP4D navigation menu use the Access control item to manage user.

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules.png)

I will perform this activity for the user named 'steward'. You may have the user of your choice.

> [!CAUTION]
> This should not be your core user, but only the additional one created for this lab.

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-1.png)

Click on the user name. On the screen use the button on the right side to 'Assign roles'

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-2.png)

Select 2 roles 'Data Engineer' and 'Data Steward' and press blue button 'Assign' at bottom-right.

The result screen should now look as following

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-3.png)

Next go to the All Catalogs from main CP4D navigation menu

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-4.png)

Click on the 'Enterprise Catalog' from the demo

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-5.png)

Switch to the Access control tab and use 'Add collaborators - Add user group' button on the right side.

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-6.png)

Find the group 'All users' and add it to the catalog access list with 'Viewer' access level by pressing button 'Add'

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-7.png)

The final screen should now hold the following new record and few other which were in the list before

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-8.png)

Pre-work done

# Creation of the Data Protection Rules

Switch to the Rules section from the main CP4D navigation menu

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-9.png)

## Creation of the Emails DP rule

Click 'Add rule - New Data Protection rule' button on the top-right.

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-10.png)

Provide the following text to the Name and Business definition fields

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-11.png)

On the next screen after If select 'Business term' from the drop-down. Then 'after contains' any in the next field select the 'Email Address' business term from the 'Customer 360 Degree View category'. Category is important here!

Next Add new condition with 'Or' case.

After If select Column name from the drop-down. Then after 'contains any' in the next field type in 'email'.

In the section What does this rule do? select Obfuscate columns, when contain Business term (from drop-down), that contains any 'Email Address'

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-12.png)

Click 'Create' button at the end

## Creation of the Phones DP rule

Use the same steps to create the Rule for protecting Telephone numbers data in the data assets.

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-13.png)

Use only one condition this time and different activity of 'Redact columns'. Please keep in mind the proper Category of the Business terms selected in each of the section.

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-14.png)

Click 'Create'

You should now be able to find your newly created rules in the list.

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-15.png)

# Demonstration of the Data Protection

Right after you've created the data protection rule it becomes active on the platform securying your data in the Catalog.

> [!IMPORTANT]
> Data Protection rules work only in the catalogs but not in the projects. In the projects you are always considered the data owner, so protection is not required.

## View of the Data Owner to the data asset

Switch to the Enterprise catalog with your current user

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-16.png)

Open the ACCOUNT_HOLDERS data asset (you either see that in the list or use the 'Find assets' string above the asset)

This asset contains both Email and Phone data. You can check that in the Overview tab with list of columns. You may need to use the paging buttons to scroll to page 2 to find the fields 'EMAIL', 'PHONE1' and 'PHONE2'.

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-17.png)

Then let's move to the tab of 'Profile'

Catalog will show profiling data on all of the columns including the emails and phone numbers.

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-18.png)

Also on the Asset tab you will see the raw data from the remote database including NON-masked data in email and phones columns.

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-19.png)

## View of the basic user to the data asset

> [!IMPORTANT]
> Relogin to the CP4D with the use you've created in the pre-work of this lab section.

Repeat the steps to reach 'Enterprise Catalog' and open the ACCOUNT_HOLDERS data asset under this basic user.

On the 'Profile' tab you will now see all the same information except for the 3 columns to which Data Protection rule has been automatically applied

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-20.png)

On the 'Asset' page you will find that 3 fields have been now masked according to the rules you've created. Emails still hold the '@' sign and domain. Phone columns now have all values been replaced by 'XXXXXXXXX'. The Shield icon accompanies now the header column name line next to each protected column.

![alt text](/Setup%20WKC%20demo%20environment/Data%20Protection%20Rules/images/DPrules-21.png)

This concludes the Data Protection rules lab section.

[Back to Setup IKC demo environment page](/Setup%20WKC%20demo%20environment/WKC_demo_setup_general_steps.md)
