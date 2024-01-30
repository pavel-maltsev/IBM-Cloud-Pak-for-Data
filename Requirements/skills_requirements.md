# Administration skills

In this section I'm to describe the basic skills which are expected to be obtained by the administrator of the platform for successfull management of the Cloud Pak for Data platform
The skills are grouped in 3 main sections:

1. deploy and maintain the platform (Openshift)
2. deploy and maintain the IBM CP4D solution
3. support of specific services of IBM CP4D platform

# Maintenance of the Openshift platform

Openshift is the core of CP4D solution and from architecture perspective that is a must have requirement no matter if you use that as on-premise, hosted or SaaS environment

If to be deployed on other than SaaS architectures, the first step is to setup Openshift cluster with all pre-requisites followed from both documentation of RedHat Openshifo and also CP4D list of supporteed minimums.

The Openshift administrator should be skilled in the following general topics:

- Managing OpenShift clusters from the command-line interface and from the web console
- Deploying applications on OpenShift from container images, templates, and Kubernetes manifests
- Troubleshooting network connectivity between applications inside and outside an OpenShift cluster
- Connecting Kubernetes workloads to storage for application data
- Configuring Kubernetes workloads for high availability and reliability
- Managing updates to container images, settings, and Kubernetes manifests of an application

and the detailed skills needed are:

1. Manage OpenShift Container Platform

- Use the command-line interface to manage and configure an OpenShift cluster
- Use the web console to manage and configure an OpenShift cluster
- Create and delete projects
- Import, export, and configure Kubernetes resources
- Examine resources and cluster status
- View logs
- Monitor cluster events and alerts
- Troubleshoot common cluster events and alerts
- Use product documentation
- Manage users and policies

2. Configure the HTPasswd identity provider for authentication

- Create and delete users
- Modify user passwords
- Modify user and group permissions
- Create and manage groups
- Control access to resources

3. Define role-based access controls

- Apply permissions to users
- Create and apply secrets to manage sensitive information
- Create service accounts and apply permissions using security context constraints
- Configure networking components

4. Troubleshoot software defined networking

- Create and edit external routes
- Control cluster network ingress
- Create a self signed certificate
- Secure routes using TLS certificates
- Configure pod scheduling

5. Limit resource usage

- Scale applications to meet increased demand
- Control pod placement across cluster nodes
- Configure cluster scaling
- Manually control the number of cluster workers
- Automatically scale the number of cluster workers

## Skills for Openshift administrator

As of 4.8.1 release of CP4D the supported OCP versions are:

- [Version 4.12](https://docs.openshift.com/container-platform/4.12/welcome/index.html)
- [Version 4.14](https://docs.openshift.com/container-platform/4.14/welcome/index.html)

which means that the skils on OCP should be up to date for the release mentioned.

## Hardware requirements

IBM Cloud Pak for Data can be deployed both on bare metal and virtualised environments, different types of the hardware (x86-64 vs Power vs z(s390x)). The skills related to management those are dependent on the selected options from the documentation:
https://www.ibm.com/docs/en/cloud-paks/cp-data/4.8.x?topic=requirements-hardware

## Storage skills and requirements

CP4D also supports the specific subset of the storages within the cluster which are listed by the link https://www.ibm.com/docs/en/cloud-paks/cp-data/4.8.x?topic=planning-storage-considerations
Those may differ on the target deployment platform, so different skills may be required. In most of the cases the NFS and Portworx storage types satisfy the typical configuration requirements.
Important information to consider is that there may be the differences of the supported storyage types by the specific CP4D services you plan to run. This information can be also found in CP4D documentation by the link https://www.ibm.com/docs/en/cloud-paks/cp-data/4.8.x?topic=requirements-storage
