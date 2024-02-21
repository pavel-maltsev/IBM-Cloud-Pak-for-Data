# Demo Assets with sample data

This chapter describes some of the demo assets which can be used to create demonstration scenarios for the topics of data connectivity, data virtualization, datastage, data lineage, etc.

> [!Important]
> Although the majority of the listed sources do not contain any restictions for the usage, please be aware that the data assets found in the internet may be the subject to copyrights and verification of asset usability for the specific use case is the responsibility of the specialist to setup the specific demo scenario.

## DVD Rental database - PostgreSQL

This database contains several tables, views, functions, etc which can be used to support the nice looking data lineage demo

Content:

    * 15 tables
    * 1 trigger
    * 7 views
    * 8 functions
    * 1 domain
    * 13 sequences

The database detailed description including the ER Model, table descriptive information, import dumps are available by [THIS LINK](https://www.postgresqltutorial.com/postgresql-getting-started/postgresql-sample-database/).

The guide for upload of the database content to external RDBMS is [PROVIDED HERE](https://www.postgresqltutorial.com/postgresql-getting-started/load-postgresql-sample-database/).

In case of questions on how to deploy standalone Postgres database you may use the folling [chapter of my github](https://github.com/pavel-maltsev/Databases/tree/main).

## Reference data and registers from UK officials

Core web page - https://www.data.gov.uk/

[Registers collections web page](https://webarchive.nationalarchives.gov.uk/ukgwa/20210104110201/https:/www.registers.service.gov.uk/)

[Reference data on github](https://github.com/openregister/registers-data-archive/tree/master)

## Reference data and registers from USA officials

More than 200K datasets can be found [here](https://catalog.data.gov/dataset?q=&sort=views_recent+desc)

## Mouse Genome DataSet - MGD

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
