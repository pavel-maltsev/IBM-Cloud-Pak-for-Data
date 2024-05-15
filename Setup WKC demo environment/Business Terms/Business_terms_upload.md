# Provision of the proper credentials to the out of the box Categories

IBM CP4D after standard deployment there are couple of pre-built categories where the sample data classes and reference data sets reside. Those are Locations and uncategorised. Before you can upload the demo content into the service under your own user, this user account should be assigned proper credentials for those existing folders.

> [!IMPORTANT]
> make sure you are logged into UI under the default 'cpadmin' user account

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary.png)

Open the 'uncategorised' category, and switch to the 'Access Control' tab

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-1.png)

Уou will see the list of the default users including cpadmin and kubeadmin with Owner category roles. Add your user account by clicking the 'Add collaborators' button on the top-right and then ''Add users'

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-2.png)

Then select the user account by clicking checkbox on the left and in the right side assign proper priveliges - 'Admin' and/or 'Owner'.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-3.png)

End your activity by clicking 'Add' on the bottom-right of the screen

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-4.png)

You should now get the confirmation message and see the added users in the 'Access Control' tab.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-5.png)

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-6.png)

Perform the same steps for the 'Locations' category

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-7.png)

# Import of the Categories sample data

Switch to the Categories list top page. Select 'Add category - Import from file' on top-right.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-8.png)

Drag-n-drop the file with categories into the pop-up window

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-9.png)

Press Next button

Select 'Replace all values' and click 'Next'

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-11.png)

In a while you should see the Import Summary screen to confirm successful upload

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-12.png)

Press 'Close'

When back to the Categories screen press 'Refresh categories' button on the right

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-13.png)

Now you should be able to view imported categores

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-14.png)

# Import of Classifications

Switch to the Classifications list

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-15.png)

Select 'Add classification - Import from file' on top-right.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-16.png)

Drag-n-drop classifications sample csv file into the pop-up window and press 'Next'

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-17.png)

Select 'Replace all values' on the next screen and press 'Next'

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-18.png)

In a while you should see the Import Summary screen to confirm successful upload

This potentially may not change the list of the classification if default sample data is the same as in the import file.

You may avoid waiting import results by closing the pop-up window.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-29.png)

# Import of policies

Switch to the Policies list

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-24.png)

If performed on fresh install this page should now not contain any of the artifacts.

Select 'Add policy - Import from file' on top-right.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-25.png)

Drag-n-drop policies sample csv file into the pop-up window and press 'Next'

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-26.png)

Select 'Replace all values' on the next screen and press 'Next'

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-27.png)

You may avoid waiting import results by closing the pop-up window.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-28.png)

Open 'Task inbox' from the CP4D menu on top-left

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-30.png)

From the task list on the 'Open' tab select 'Publish Classifications' task, review details about import and press 'Publish' button on the right.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-31.png)

When doing publish you may want to leave some comments of the approval. This is optional. Press 'Publish'.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-34.png)

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-32.png)

Perform the same action for the 'Publish Policies' task

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-33.png)

When doing publish you may want to leave some comments of the approval. This is optional.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-34.png)

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-35.png)

There should be no Open tasks left now

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-36.png)

# Import of business terms

Switch to the Business terms list

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-19.png)

If performed on fresh install this page should now not contain any of the artifacts.

Select 'Add business term - Import from file' on top-right.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-20.png)

Drag-n-drop business terms sample csv file into the pop-up window and press 'Next'

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-21.png)

Select 'Replace all values' on the next screen and press 'Next'

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-22.png)

On the summary page there could be some errors. In case of errors mention 'User ID XXXXXXX is not valid...' you may skip those errors. User IDs specified are linked to some of the business terms as owners on the original pllatform. Those IDs may not be present on the current platform but the import would be successful in this case.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-23.png)

You may now as asked go to the task and perform all the confirmations of the imported tasks

In the 'Task inbox' you would now see the single task for 'Publish business terms'

If you select the checkbox left to the task the screen would change from detailed view of the task to the summary - that is the option to perform several operations at one click.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-37.png)

Press Publish, do not add comments, then press 'Publish' again.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-38.png)

# Import of Rules

Rules are dependent on the business terms, policies and classification, so those should be imported after the previous imports are complete.

Switch to the Rules list

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-39.png)

If performed on fresh install this page should now not contain any of the artifacts.

Select 'Add rule - Import from file' on top-right.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-40.png)

Drag-n-drop rules sample csv file into the pop-up window and press 'Next'

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-41.png)

Select 'Replace all values' on the next screen and press 'Next'

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-42.png)

Import completes succesfully. Press the button to 'go to task'

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-43.png)

Same as before, select the Open task 'Publish Governance rules' and press button 'Publish'.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-44.png)

Don't leave comments and press 'publish' again

# Validation of the imported content

We will now perform the validation of uploaded business terms.

Select 'Business terms' from the main CP4D menu

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-45.png)

In the search field type 'customer'

In the search results find the Customer business term. There are 2 of those in different categories. Click on the one which is located in the 'Customer 360 Degree View' Primary Category.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-47.png)

You would reveal the Customer business term in approved state with all detailed data filled in.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-48.png)

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-49.png)

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-50.png)

Press the 'Explore relationships' button right from the business term name

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-51.png)

The screen will contain only Customer business term firstly. Use the left menu to show also 'Category' and 'Classification' Artifact types

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-52.png)

When also selecting the 'Business terms' from the list you will see more objects on the graph. Select the 'Synonym' relationship from 'Relationships' types list on the right. The graph will highlight the Synonym relationship between 2 'Customer' terms in different Primary categories you have seen previously also on the serach results screen.

![alt text](/Setup%20WKC%20demo%20environment/Business%20Terms/images/import_glossary-53.png)

This concludes the Glossary artifacts import lab section.

[Back to Setup IKC demo environment page](/Setup%20WKC%20demo%20environment/WKC_demo_setup_general_steps.md)
