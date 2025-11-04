# Introduction

In this article I would provide the guidance on usage of REST API calls in backend integration with IBM Product Master solution for Master data management.

Master data management solutions are the heart of the core data asset management initiative, which allows companies to build a single view on the Master data across the enterprise. For the specific centralized management style of Master data the IBM Product Master solution is one of the best available nowadays in the market. The solution from IBM does not focus on working only within landscape of IBM vendor SW, but instead allows to manage the data for unified business processes spread through multi-vendor solutions of any Enterprise.

# IBM Product Master concepts

IBM Product Master is the solution which can flexibly fit into any industry use case, enabling the same master data management capabilities, no matter if those are for domains of Governmental, Healthcare, Banking, Retail, Telecom or other organizations.

This is possible due to the specific data management and data storage tiers' design of the solution. The concept of "meta-over-meta" keeps the database metadata structures intact, while the services tier allows administrators of the solution to build the Master catalogs with all required hierarchies and item metastructures from scratch and only with web-based UI. It never requires the solution administrators to touch or modify the actual storage reporisoty model, create tables, views or indexes for new or updated master objects.

The flexibility for the solution administration is facilitated by the powerful engine of IBM Product Master which coverts the business-style activities of the administrator to the technical commands for tuning the solution. And then when the Business users work with their domains, the complex Master objects which may be of 100ds of attributes, nested attribute structures, multi-occuring attribute groups or any data type and more, are automatically converted with split of technical metadata (schema) from data content itself. When IPM lands the data into the repository (which is a classical DB2 or Oracle database) the data can't be easily read by SQL queries as the same table of Master Items holds the numerous items from any domain catalogs of solution.

This leads to the major block for the data integration initiatives when ETL specialists would like to bypass the security rules and ease the data retrieval from the solution directly by connecting ETL to database, instead of API.

The proper method of data retrieval from IBM PM solution is via APIs. Those allow to repeate any operation available in web-based user UI and place the data consumption under control.

# IBM Product Master APIs

## API documentation

The definition of APIs can be found for the specific version of IBM PM in the knowledge center published Swagger file.

https://www.ibm.com/docs/en/product-master/14.0.0?topic=apis-product-master-rest-api-swagger-files

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image.png)

## API usage principles

The usage of APIs require the same sequence of actions as business or administrative operations in web-based UI of the soluiton.

When working in Web UI the user firstly has to authenticate himself with 3 parameters which are User name, Password and Company (tenant of the soluiton).

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-1.png)

In this demo I would use the RESTed Client in Firefox browser to perform the REST calls, but you can use any other tool to repeat same actions.

The authentication REST call is /v1/login. it requires several parameters, one of which is the Base 64 encoded username:password string.

The Basic authentication will not accept the plain form of user name and password, so this encoded string has to be acquired, e.g. from the following portal https://www.base64encode.org/

In the top line switch to the ENCODE

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-2.png)

The place your username:password pair into the top box and press ENCODE button. The values to be encoded are CASE SENSITIVE!

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-3.png)

For the pair Admin:trinitron the Encoded string would be QWRtaW46dHJpbml0cm9u

In your own environment you will get own value for your credentials.

## API usage samples

### Authentication

Use the REST API call to retrieve the X-AuthToken

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-4.png)

The response will contain the value for the X-AuthToken in on of the Header fields

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-5.png)

The Body of response contains the whole list of metaobjects and features of IPM you will have access to using these credentials

You may use the /v1/authToken/validate method to check if retrieved value of the Token is still valid

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-6.png)

If validation is successful you are now ready to use the token for any other API calls your user has permissions setup.

### List Catalogs

Lets try retrieval of the Catalog list of Product Master

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-7.png)

The body of the response contains all the details about not only Catalog names, but also information about Access Groups, Hierarchies and Views of specific Catalog

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-8.png)

What is important from the data retrieved are not the names, but the IDs of the objects you will use later for operations on those Catalogs and Hierarchies.

### List items of specific Catalog

As you have now the ID of the catalog, you may use that for getting the list of the items of the catalog.
What is important to remember here is that the API calls use the same security limitations as the usage of the platform from web-based UI. It means that if your user can't retrieve some of the items or attributes in UI, the same limitations will be applied to response of API call.

Here is the sample of item retrieval for the catalog with ID=172826

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-9.png)

The content retrieved is in a form of JSON array where all the master items are held under the section of "entryInfoList"

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-10.png)

As you can see the data displayed in the response is given in the same way as coded in the output style of specification. The only exception is the attribute names.

### Specfications' metadata operations

Attribute names in IBM Product Master can be localized to several locales depending on the user preferences. This allows users to check the Business UI on their own language, while the technical metadata likely is still managed in English.

This means that the attribute names are also IDs of the specific nodes of item specifications.

For integration purposes that is not a big issue as JSON parser can operate with codes in the JSON schema file instead of the names. but if the goal is to understand what are the actual attribute names behind the code, those could be easily retrieved by browsing the specification, firstly by name

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-11.png)

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-12.png)

and then the specification content by it's ID

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-13.png)

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-14.png)

...

![alt text](/IBM%20Product%20Master%20REST%20API%20sample%20use/images/image-15.png)

## Using IBM Product Master REST APIs from IBM DataStage ETL

For those familiar with IBM DataStage ETL tool, the same operations could be performed by using the Hierarchical Data stage on the canvas.

Few core principles of that operation are:

1. Best is to use the technical user account created on IBM PM side dedicated for ETL integrations by Product Master Administrators. This will provide you the ease of retrieval of Base64 authenticated pair of credentials
2. The whole set of the calls including authentication is better to be done within same Hierarchical Data stage step-by-step. If not required by specific reasons, do not split the processing between atomic HD stages on the canvas. This would lead to the issues with extra RAM consumption and also bring complexity with transfer of the parameters from response one call to request of the next one.
3. The Authentication method on the REST call operation should be set to '''NONE'''. The headers should contain your authentication parameters instead.
4. As typical for Hierarchy stage operations, you should keep to the proper sequence of the calls listed from top to the bottom of the left pane list, otherwise you won't be able to refer to the specific fields of the response which is not yet executed.
5. Jointly with Product Master admins prepare the JSON schema files in order to parse the retrieved data to the normalized format.
