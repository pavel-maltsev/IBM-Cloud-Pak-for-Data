# Objectives

Tips regarding the metadata export files from Informatica Powercenter content expected for the Manta successful lineage import

## Structure of the export

Export should contain serveral files and folders included in the same ZIP file

1. Connection details in ifpcConnectionDefinition.prm

in case of this file missing, the structure of the workflows would be incomplete and several stages would be missing even inside the transformation flow.

2. Services details in ifpcServiceSettings.prm
3. pmrep.cnx file
4. Parameter files exported in the subfolder
5. Workflow and Worklet files exported in the subfolder "workflow"

this folder may contain 10ns of the files stored by the path /workflow/SomeIPCProjectName/\*.xml

## Sample content of ifpcConnectionDefinition.prm

```
[Oracleconnectionname]
Type=Oracle
Connection_String=someservername.com
User_Name=user1

[MSSQLconnectionname]
Type=MSSQL
Server_Name=someservername.com,0000
Database_Name=databasename
User_Name=user1

[DB2connectionname]
Type=DB2
Connection_String=localservername
User_Name=user1

[ODBCconnectionname1]
Type=ODBC
Connection_String=odbcserver.com
User_Name=user1

[ODBCconnectionname2]
Type=ODBC
Connection_String=localservername
User_Name=user1

[SAPBWconnectionname]
Type=SAP BW
Database_Name=databasename
User_Name=user1

[SAPR3connectionname]
Type=SAP R3
Database_Name=databasename

[SAPRFCBAPIconnectionname]
Type=SAP RFC/BAPI Interface
Database_Name=databasename

[WebServicesConsumerconnectionname1]
Type=Web Services Consumer
Database_Name=XX.XX.XX.XX:0000/FFF/YYY_ZZZ/somestring?wsdl

[WebServicesConsumerconnectionname2]
Type=Web Services Consumer
Database_Name=http://XX.XX.XX.XX/somestring.asmx

[HttpTransformationconnectionname]
Type=Http Transformation
Database_Name=https://somedomain.com/someurlstring

[Salesforceconnectionname]
Type=Salesforce
Database_Name=databasename

[FTPconnectionname1]
Type=FTP
Server_Name=someserver.com
Schema_Name=/some/file/path/
User_Name=user1

[FTPconnectionname2]
Type=FTP
Server_Name=XX.XX.XX.XX
Schema_Name=/
User_Name=user1
```

## Sample content of ifpcServiceSettings.prm

```
[SOMENAME]
$PMRootDir=/some/path
$PMSessionLogDir=$PMRootDir/SessLogs
$PMBadFileDir=$PMRootDir/BadFiles
$PMCacheDir=/some/path/cache
$PMTargetFileDir=$PMRootDir/TgtFiles
$PMSourceFileDir=$PMRootDir/SrcFiles
$PMExtProcDir=/some/path/ExtProc
$PMTempDir=/some/path
$PMWorkflowLogDir=/some/path/WorkflowLogs
$PMLookupFileDir=$PMRootDir/LkpFiles
$PMStorageDir=/some/path/Storage
```
