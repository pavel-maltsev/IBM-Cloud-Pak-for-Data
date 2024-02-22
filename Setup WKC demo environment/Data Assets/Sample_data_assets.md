# 1 Demo Assets with sample data

This chapter describes some of the demo assets which can be used to create demonstration scenarios for the topics of data connectivity, data virtualization, datastage, data lineage, etc.

> [!Important]
> Although the majority of the listed sources do not contain any restictions for the usage, please be aware that the data assets found in the internet may be the subject to copyrights, and therefore verification of asset usability for the specific use case is the responsibility of the specialists who setup the specific demo scenario.

## 1.1 DVD Rental database - PostgreSQL

This database contains several tables, views, functions, etc which can be used to support the nice looking data lineage demo

Content:

- 15 tables
- 1 trigger
- 7 views
- 8 functions
- 1 domain
- 13 sequences

The database detailed description including the ER Model, table descriptive information, import dumps are available by [THIS LINK](https://www.postgresqltutorial.com/postgresql-getting-started/postgresql-sample-database/).

The guide for upload of the database content to external RDBMS is [PROVIDED HERE](https://www.postgresqltutorial.com/postgresql-getting-started/load-postgresql-sample-database/).

In case of questions on how to deploy standalone Postgres database you may use the following [chapter of my github](https://github.com/pavel-maltsev/Databases/tree/main).

## 1.2 Reference data and registers from UK officials

Core web page - https://www.data.gov.uk/

[Registers collections web page](https://webarchive.nationalarchives.gov.uk/ukgwa/20210104110201/https:/www.registers.service.gov.uk/)

[Reference data on github](https://github.com/openregister/registers-data-archive/tree/master)

## 1.3 Reference data and registers from USA officials

More than 200K datasets can be found [here](https://catalog.data.gov/dataset?q=&sort=views_recent+desc)

## 1.4 Mouse Genome DataSet - MGD

Large volume database with medical experiments data can be found [here](https://www.informatics.jax.org/software.shtml)

Data Files for Download

1. Reports for download - Public reports via FTP

- Over 65 data files are generated weekly and available for download.
- See [MGI Data and Statistical Reports](https://www.informatics.jax.org/downloads/reports/index.html) for the list of reports and their field descriptions.

2. Database Dumps via FTP

```
Database backups are available from our ftp site at: http://www.informatics.jax.org/downloads/database_backups/

Backups are available for MySQL 5.0.67 and PostgreSQL 9.3.5.

These backups assume you have a compatible version of MySQL or PostgreSQL installed, that you have created a database, and that you have adequate user permissions to load that database.

Backups are updated on Mondays between 12:30 AM and 1:30 AM EST, so please avoid downloading the files during that time, or your file may not download successfully.

To load the MySQL backup:

First, uncompress it: gunzip mgd.mysql.dump.gz

Then load it: mysql -e "mgd.mysql.dump" databaseName

To load the PostgreSQL backup:

Load it: pg_restore -c -d databaseName -j jobCount -O -h host -U user mgd.postgres.dump-Fc

jobCount should be an integer, representing the number of subprocesses. If you have a multiprocessor machine, using a jobCount between 2 and 4 will likely speed your restore significantly.
```

> [!TIP]
> Additional steps needed to create a role at new DB. Without this the upload will fail
>
> ```
> CREATE DATABASE mgd;
> CREATE ROLE mgd_public WITH PASSWORD ‘mgd_public’
> pg_restore -c -d mgd -j 2 -O -U postgres mgd.postgres.dump -Fc
> ```

## 1.5 Postgres Flights Database

https://postgrespro.com/community/demodb

### Information from authors:

```
You can use this database for various purposes, such as:
learning SQL language on your own
preparing books, manuals, and courses on SQL
showing PostgreSQL features in stories and articles

When developing this demo database, we pursued several goals:
Database schema must be simple enough to be understood without extra explanations.
At the same time, database schema must be complex enough to allow writing meaningful queries.
The database must contain true-to-life data that will be interesting to work with.

This demo database is distributed under the PostgreSQL license. Current version is 15.08.2017.
```

### Installation

The demo database is available at edu.postgrespro.com in three flavors, which differ only in the data size:

[demo-small-en.zip](https://edu.postgrespro.com/demo-small-en.zip) (21 MB) — flight data for one month (DB size is about 300 MB)

[demo-medium-en.zip](https://edu.postgrespro.com/demo-medium-en.zip) (62 MB) — flight data for three months (DB size is about 700 MB)

[demo-big-en.zip](https://edu.postgrespro.com/demo-big-en.zip) (232 MB) — flight data for one year (DB size is about 2.5 GB)

The small database is good for writing queries, and it will not take up much disk space. The large database can help you understand the query behavior on large data volumes and consider query optimization.

The files include an SQL script that creates the demo database and fills it with data (virtually, it is a backup copy created with the pg_dump utility). The owner of the demo database will be the DBMS user who runs the script. For example, to create the small database, run the script as the user postgres by means of psql:

### Industry Accelerators by IBM

Industry Accelerators by IBM also provide some sample data and those may be found by [the link](https://accelerator.ca.analytics.ibm.com/bi/?perspective=authoring&pathRef=.public_folders%2FIBM%2BAccelerator%2BCatalog%2FIBM%2BAccelerator%2BCatalog&id=iF7884CDF92664BD0868E68D4D06AB5D0&objRef=iF7884CDF92664BD0868E68D4D06AB5D0&action=run&format=HTML&cmPropStr=%7B%22id%22%3A%22iF7884CDF92664BD0868E68D4D06AB5D0%22%2C%22type%22%3A%22report%22%2C%22defaultName%22%3A%22IBM%20Accelerator%20Catalog%22%2C%22permissions%22%3A%5B%22execute%22%2C%22read%22%2C%22traverse%22%5D%7D).

Few samples which are included:

- Automotive Industry Sample Data
- Retail Industry Sample Data
- Travel and Transportations Industry Samples Data
- Profeccional Services Industry Sample Data

and many more
