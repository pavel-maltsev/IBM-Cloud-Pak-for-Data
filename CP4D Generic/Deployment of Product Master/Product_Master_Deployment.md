# IBM Product Master sample deployment demo

> [!CAUTION]
> This article is still in work. Instructions as of now do not cover the full deployment process.

The deployment of Product Master on CP4D v5 is performed as a sequencial install of several components in a proper order. In many of the steps prior to continue with deployment of next component the specific configuration activiites on the previous one have to be performed.

```mermaid
graph TD;
    A[Getting configuration of the cluster]-->B[Deployment of the cpd-cli on the bastion node];
    B-->C[Confugration of environment variables];
    A-->D[Deployment of DB2 service on CP4D];
    A-->F[Deployment of IKC service on CP4D];
    C-->M[Product Master Service deployment];
    D-->E[Creation and configuration of PIMDB on DB2];
    F-->G[Creation and configuration of Catalog and user token on IKC];
    G-->M;
    E-->M;
    A-->H[Deployment of OpenSearch service on Openshift];
    H-->M;
    M-->O[Product Master Instance deployment];
    O-->P[Post-install configuration of Product Master];
```

# Confugration of environment variables

The bastion node or your local laptop you plan to use for deployment of Product Master or any other services using CPD-CLI should use the proper set of Environment variables in order to use the scripts from documentation with minimum changes.

Build the following file as per my example below. For basic deployment as per this demo you don't actually need to complete all the lines, but only ones as shown.

- OCP_URL - Get this URL from the OC login command in your Openshift console. It may be on different port vs one mentioned in documentation. In my case it's `6443`.
- OCP_USERNAME - `kubeadmin` or other similar user with administrative privileges on your cluster
- OCP_PASSWORD - password of the same user
- OCP_TOKEN - get this value from the OC login command in your Openshift console. In my case that starts with `sha256~`.

- PROJECT_CPD_INST_OPERATORS - get this value from the list of the projects in Openshift console. In my case that is `cpd-operators`.
- PROJECT_CPD_INST_OPERANDS - get this value from the list of the projects in Openshift console. In my case that is `cpd`.

Get Operators name from this screen in your environment:

![alt text](images/Env_var.png)

Operands is the name of the project you have CP4D deployed. This can be refealed by searching for Pods of other your deployed services (column Namespaces), e.g. IKC you will integrate Product Master Metadata information with later on.

![alt text](images/Env_var-1.png)

- STG_CLASS_BLOCK - get this value from the list of the storage classes in Openshift console. In my case that is `ocs-storagecluster-cephfs`.
- STG_CLASS_FILE - get this value from the list of the storage classes in Openshift console. In my case that is `ocs-storagecluster-cephfs`.

![alt text](images/Env_var-2.png)

- IBM_ENTITLEMENT_KEY - the Entitlement key for connection to IBM repo.

```
#===============================================================================
# Cloud Pak for Data installation variables
#===============================================================================

# ------------------------------------------------------------------------------
# Client workstation
# ------------------------------------------------------------------------------
# Set the following variables if you want to override the default behavior of the Cloud Pak for Data CLI.
#
# To export these variables, you must uncomment each command in this section.

# export CPD_CLI_MANAGE_WORKSPACE=<enter a fully qualified directory>
# export OLM_UTILS_LAUNCH_ARGS=<enter launch arguments>


# ------------------------------------------------------------------------------
# Cluster
# ------------------------------------------------------------------------------

export OCP_URL=https://api.XXXXXXX.ocp.techzone.ibm.com:6443
export OPENSHIFT_TYPE=self-managed
export IMAGE_ARCH=amd64
export OCP_USERNAME=kubeadmin
export OCP_PASSWORD=Place you value here
export OCP_TOKEN=sha256~XXXXXX-Place you value here
export SERVER_ARGUMENTS="--server=${OCP_URL}"
# export LOGIN_ARGUMENTS="--username=${OCP_USERNAME} --password=${OCP_PASSWORD}"
export LOGIN_ARGUMENTS="--token=${OCP_TOKEN}"
export CPDM_OC_LOGIN="cpd-cli manage login-to-ocp ${SERVER_ARGUMENTS} ${LOGIN_ARGUMENTS}"
export OC_LOGIN="oc login ${OCP_URL} ${LOGIN_ARGUMENTS}"


# ------------------------------------------------------------------------------
# Proxy server
# ------------------------------------------------------------------------------

# export PROXY_HOST=<enter your proxy server hostname>
# export PROXY_PORT=<enter your proxy server port number>
# export PROXY_USER=<enter your proxy server username>
# export PROXY_PASSWORD=<enter your proxy server password>


# ------------------------------------------------------------------------------
# Projects
# ------------------------------------------------------------------------------

# #export PROJECT_CERT_MANAGER=<enter your certificate manager project>
# #export PROJECT_LICENSE_SERVICE=<enter your License Service project>
# #export PROJECT_SCHEDULING_SERVICE=<enter your scheduling service project>
# export PROJECT_IBM_EVENTS=<enter your IBM Events Operator project>
# export PROJECT_PRIVILEGED_MONITORING_SERVICE=<enter your privileged monitoring service project>
export PROJECT_CPD_INST_OPERATORS=cpd-operators
export PROJECT_CPD_INST_OPERANDS=cpd
# export PROJECT_CPD_INSTANCE_TETHERED=<enter your tethered project>
# export PROJECT_CPD_INSTANCE_TETHERED_LIST=<a comma-separated list of tethered projects>



# ------------------------------------------------------------------------------
# Storage
# ------------------------------------------------------------------------------

export STG_CLASS_BLOCK=ocs-storagecluster-cephfs - Place you value here instead
export STG_CLASS_FILE=ocs-storagecluster-cephfs - Place you value here instead

# ------------------------------------------------------------------------------
# IBM Entitled Registry
# ------------------------------------------------------------------------------

export IBM_ENTITLEMENT_KEY=eyJhbGciOiJdivsboksASQUuk-Place you value here instead - that is the long string you own from IBM.


# ------------------------------------------------------------------------------
# Private container registry
# ------------------------------------------------------------------------------
# Set the following variables if you mirror images to a private container registry.
#
# To export these variables, you must uncomment each command in this section.

# export PRIVATE_REGISTRY_LOCATION=<enter the location of your private container registry>
# export PRIVATE_REGISTRY_PUSH_USER=<enter the username of a user that can push to the registry>
# export PRIVATE_REGISTRY_PUSH_PASSWORD=<enter the password of the user that can push to the registry>
# export PRIVATE_REGISTRY_PULL_USER=<enter the username of a user that can pull from the registry>
# export PRIVATE_REGISTRY_PULL_PASSWORD=<enter the password of the user that can pull from the registry>


# ------------------------------------------------------------------------------
# Cloud Pak for Data version
# ------------------------------------------------------------------------------

export VERSION=5.0.0


# ------------------------------------------------------------------------------
# Components
# ------------------------------------------------------------------------------

export COMPONENTS=ibm-cert-manager,ibm-licensing,scheduler,cpfs,cpd_platform
# export COMPONENTS_TO_SKIP=<component-ID-1>,<component-ID-2>


# ------------------------------------------------------------------------------
# watsonx Orchestrate
# ------------------------------------------------------------------------------
# export PROJECT_IBM_APP_CONNECT=<enter your IBM App Connect in containers project>
# export AC_CASE_VERSION=<version>
# export AC_CHANNEL_VERSION=<version>
```

# Deployment of IKC service on CP4D

IBM Product Master integrates with IBM Knowledge Catalog service for the metadata management purposes. This integration is optional.

For this specific excersise I'm using the previously deployed IKC service with all additional options set to `true` and the `small` tier configuration. But the minimum configuration is also supported.

# Creation and configuration of Catalog and user token on IKC

For this demo I've created the catalog with the name `Enterprise Catalog` and will use the `admin` user as the owner of this catalog for integration with Product Master

![alt text](images/IKC_Conf.png)

The API key can be received by openning `Profile and settings` page of the `admin` user

![alt text](images/IKC_Conf-1.png)

Then clicking the `API key` button on top-right.

![alt text](images/IKC_Conf-2.png)

# Deployment of DB2 service on CP4D

For this demo I've used the Deployer pipeline documented on the IBM github. Since the Cognos Analytics service also required repository, it has been deployed together with instance. This instance you will see later on this guide and I will show how to distinguish that one from the one we will work with.

ConfigMap part for DB2 originally used has been the following:

```
- name: db2
  description: Db2 OLTP
  size: small
  instances: - name: ca-metastore
  metadata_size_gb: 20
  data_size_gb: 20
  backup_size_gb: 20
  transactionlog_size_gb: 20
  state: installed
```

If you have not deployed other services with DB2 requirement on this cluster, then simply the initial the DB2 service in `small` tier size. Then you may create the instance in CP4D web UI.

```
- name: db2
  description: Db2 OLTP
  size: small
  state: installed
```

# Creation and configuration of PIMDB on DB2

Providing the repository tier is must-have pre-requisite for the deployment of IBM Product Master. According to current documentation, the supported repository types are Oracle and DB2. Since in this setup we deploy the Product Master service on Cloud Pak for Data, the default should be deployment of the DB2 for these purposes, but still Oracle outside of the cluster remains the valid option. For Oracle related pre-requisites please refer to documentation of PM and Oracle.

Below on the screenshot are parameters which I've used for the base setup of the demo instance of DB2. The proper sizing exercise should be performed for production deployment jointly with IBM technical team.

![alt text](images/PIMDB.png)

Get information about CP4D project you install service into from oc web console. At my cluster it's `cpd`.

![alt text](images/PIMDB-1.png)

Get information about installed DB2 instance. The cluster may have several instanced deployed. E.g. in my case the second instance stands for repository of Cognos Analytics - another service on CP4D.

![alt text](images/PIMDB-2.png)

The one you need is by name similar to the Deployment ID from the first screenshot.

![alt text](images/PIMDB-3.png)

Get the OC login command from Web-UI.

![alt text](images/PIMDB-4.png)

Now open the terminal and use the login command to get to openshift cluster. Top-right corner, `Copy login command`, on the next screen - `Display token`.

![alt text](images/PIMDB-5.png)

Currently message shows you are in the `default` project. Switch to the proper project (`cpd` in my case).

![alt text](images/PIMDB-6.png)

Get to the DB2 pod of the name located previously.

![alt text](images/PIMDB-7.png)

Get the password for the DB2 instance you've created earlier. You will need that later for creation of YAML file

![alt text](images/PIMDB-8.png)

Switch to the `db2inst1` user.

![alt text](images/PIMDB-9.png)

Get created database name.

![alt text](images/PIMDB-10.png)

Get the service name of the database that is to be used as IBM Db2 host database name in app-secret.

![alt text](images/PIMDB-11.png)

Defatul port number is 50000 - in my case it's line 2.

Start creation of the TABLESPACES as per documentation in the pod used earlier. Switch to the db2 pod and verify that PIMDB is present there.

![alt text](images/PIMDB-12.png)

Connect to PIMDB.

![alt text](images/PIMDB-13.png)

Start execution of the scripts from documentation - as is, no modification is required

![alt text](images/PIMDB-14.png)

![alt text](images/PIMDB-15.png)

![alt text](images/PIMDB-16.png)

![alt text](images/PIMDB-17.png)

Make sure that execution is successfull. If not, debug that with your DB2 administrator.

# Install OpenSearch

## Before you install OpenSearch

Before installing Opensearch make sure you have helm package deployed.

## Create persistent volumes.

Change the namespace to project name of your CP4D cluster, same as use proper name of the Storage Class which can be looked up in the web-UI of Openshift

![alt text](images/OSearch_PVC-3.png)

![alt text](images/OSearch_PVC.png)

Create first Pesistent Volume Claim using the code below

```
cat <<EOF| oc apply -f -
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
name: opensearch-cluster-master-opensearch-cluster-master-0
namespace: cpd
spec:
accessModes:

- ReadWriteOnce
  resources:
  requests:
  storage: 2Gi
  storageClassName: ocs-storagecluster-cephfs
  volumeMode: Filesystem
  EOF
```

![alt text](images/OSearch_PVC-1.png)

Repeate this tep 2 more times for creation of total 3 Pesistent Volume Claims. Only change is the number in the name attribute.

![alt text](images/OSearch_PVC-2.png)

## OpenSearch configuration and deployment

Using terminal login to Openshift and check if any Opensearch repository installed

![alt text](images/OSearch_Conf.png)

In my case there is no such repository, then it should be deployed and presence should be validated.

![alt text](images/OSearch_Conf-1.png)

![alt text](images/OSearch_Conf-2.png)

Update the repository

![alt text](images/OSearch_Conf-3.png)

Deploy OpenSearch

![alt text](images/OSearch_Conf-4.png)

Check pods status intil 3 master pods of OpenSearch would be running. This can be done using Terminal.

![alt text](images/OSearch_Conf-5.png)

Or same can be checked in Openshift web based console

![alt text](images/OSearch_Conf-6.png)

Check the service up and running

![alt text](images/OSearch_Conf-7.png)

Follow the documentation to create server and client certificates with information about your organization. This step is straight forward described in documentation. Replace XXXX values to your own and make notes about those for further steps.

Finalize creation of the certificate

![alt text](images/OSearch_Conf-8.png)

Before copy of certificate to the containers make sure you have the proper folder strucuture on each of them. If not, create required folders by connecting to each container directly.

![alt text](images/OSearch_Conf-9.png)

Copy the certificate to the created folders

![alt text](images/OSearch_Conf-10.png)

Edit ConfigMap of the Opensearch

![alt text](images/OSearch_Conf-11.png)

In the top "data" section add the line from documentation for max_clause_count

![alt text](images/OSearch_Conf-12.png)

In the Security section remove default parameters starting from http line down including line with certificate ssignature parameters.

![alt text](images/OSearch_Conf-13.png)

Replace that with the data from documetnation and the parameters of your own certificate you've noted few steps above.

![alt text](images/OSearch_Conf-14.png)

Restart the pods of OpenSearch either by changeing the Replicas number as per documentation first to 0 then back to 3. These steps are described in documentation.

Alternative to terminal is to simply delete those 3 pods from web-console. Openshift will automatically restore those as per configuration and number of replicas, but the pod creation will happen with already new parameters you've changed

![alt text](images/OSearch_Conf-15.png)

Wait until all the pods will be brought back up.

![alt text](images/OSearch_Conf-16.png)

OpenSearch setup has been complete.

---

# Secret creation

Prepare the Secret for IBM Product Master deployment. Using the "+" in the top-right of Openshift console create an import of the YAML file with details of pre-requisites you've installed on the cluster. You will need the parameters for DB2 connection, IKC Catalog details and Opensearch configruation. Last one is taken from documentation page in the knowledge center. Last the OpenSearch Certificate should be joined as a single line, no quotes or separator (as per screenshot).

![alt text](images/Secret_YAML.png)

---

# Deployment of Product Master

## Docker deployment

Docker is a must-have pre-req for the CP4D deployment, so it should be installed on the workstation you will be deploying Product Master from.

If not installed, switch to root and perfom deployment. Here is the sample for RHEL OS.

![alt text](images/PM_deployment.png)

## CPD_CLI deployment

Before you begin, make sure you have setup the CPD-CLI.

Solution guide can be found here `https://www.ibm.com/docs/en/cloud-paks/cp-data/5.0.x?topic=cli-installing-cpd`

The distributive can be downloaded from this link `https://github.com/IBM/cpd-cli/releases`

Switch back to admin user, create cpd-cli directory in your home folder.

Then download cpd-cli distributive using wget command from the URL you will find in documentation for your OS by the links above

![alt text](images/PM_deployment-1.png)

Unpack the distributive from Tar

![alt text](images/PM_deployment-2.png)

Make sure you have at the same location the file of Environment variables, which has been created on the early steps.

Source those variables.

![alt text](images/PM_deployment-3.png)

Add cpd-cli to the PATH.

![alt text](images/PM_deployment-4.png)

Next step is to log in to the cluster. If you have Environment variable set correctly, then the default command from the documentation script should work

![alt text](images/PM_deployment-5.png)

![alt text](images/PM_deployment-6.png)

Initiate the deployment of Product Master Service

![alt text](images/PM_deployment-7.png)

The run of apply-olm should finish successfully

![alt text](images/PM_deployment-8.png)

Start creation of Custom Resource for the Product Master

![alt text](images/PM_deployment-9.png)

Action should be finished successfully

![alt text](images/PM_deployment-10.png)

Verify the status

![alt text](images/PM_deployment-11.png)

All good so far!

Product Master service has been deployed, but it requires instance for operation. Documentation explains the steps in the Post-install section

## Product Master instance creation

The instance creation has the dependency currently not well described in the documentation. That is the finalization of DB2 related activities intil completion of the Instance creation.

The run of instance creation is a simple command from documentation. Again, if you have proper environment variables file applied, this should be a copy-paste from documentation.

![alt text](images/PM_deployment-12.png)

The issue you may face is that at some point of time the process would be stuck for waiting for resolving some internal dependencies

![alt text](images/PM_deployment-13.png)

If not acting before that, then you will get the error in couple of hours and the instance deployment will be failed

![alt text](images/PM_deployment-14.png)

The possible reason for that is that not all the actions on DB2 repository has been properly complete.

When checking the pods of Product Master you can see that the Scheduler and Workflow pods are not ready and failing.

![alt text](images/PM_deployment-15.png)

Use the steps from documentation Post-install DB2 section to fix the issue.

Connect to the Product Master admin pod. Source the bash_profile. Then switch to the /opt/MDM/bin folder

![alt text](images/PM_deployment-16.png)

Run the ./test_db.sh script

![alt text](images/PM_deployment-17.png)

In my case it showed that connection is failed. Exit the pod.

![alt text](images/PM_deployment-18.png)

Get to the pod of DB2 repository with PIMDB repository

![alt text](images/PM_deployment-19.png)

Check that Database is up and running

![alt text](images/PM_deployment-20.png)

Quit from db2 and exit the pod

![alt text](images/PM_deployment-21.png)

Login back to Product Master admin pod

![alt text](images/PM_deployment-22.png)

Source the bash_profile same way as last time and then switch to the foler /opt/MDM/bin/db

Locate the create_schema.sh script and run it. Confirm that you want the execution to proceed as per screenshot

![alt text](images/PM_deployment-23.png)

Wait for the script to finish. There may be the error related to some rollback, but overal execution will pass OK.

Switch to /opt/MDM/bin folder (1 level up)

Run test_db.sh command again

now it should be finished successfully

![alt text](images/PM_deployment-24.png)

Now exit the Product master admin pod and check the status of the all Product Master pods

![alt text](images/PM_deployment-25.png)

As you can see, even without the need to restart the instance creation, all the pods, including the ones of scheduler and workflow, are Running 1/1

Job done!

---

# Verification of instance

Check if Product Master pods are all Running at 1/1 configuration.

```
oc get pods |grep productmaster
```

![alt text](images/Instance_verif.png)

```
oc get ProductMaster productmaster-cr -o jsonpath='{.status.productmasterStatus} {"\n"}'
```

![alt text](images/Instance_verif-1.png)

To get the access to the platform the proper URL link should be used. This can be revealed using the `oc get routes` command

![alt text](images/Instance_verif-2.png)

First 2 lines in resonse in my case stand for URLs to the Persona and Admin UIs.
