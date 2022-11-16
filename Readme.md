TechZone Automation - Cloud Pak for Integration 2022.2.1 - Automation for AWS, Azure, and IBM Cloud
Created with Techzone Accelerator Toolkit

Change Log
08/2022 - Added CP4I version, fixed broken links
07/2022 - Initial Release
This collection of Cloud Pak for Integration terraform automation layers has been crafted from a set of Terraform modules created by the IBM GSI Ecosystem Lab team part of the IBM Partner Ecosystem organization. Please contact Matthew Perrins mjperrin@us.ibm.com, Sean Sundberg seansund@us.ibm.com, Andrew Trice amtrice@us.ibm.com, Gowdhaman Jayaseelan gjayasee@in.ibm.com, Keshav Anand keshav.anand@in.ibm.com, Vijay K Sukthankar vksuktha@in.ibm.com or Jyoti Rani jyotirani10@in.ibm.com for more details or raise an issue on the repository.

The automation will support the installation of Cloud Pak for Integration on three cloud platforms (AWS, Azure, and IBM Cloud). As an initial effort it was decided to choose the key & essential capabilities such as Integration PlatformNavigator, API connect, App Connect Enterprise, Mq, EventStreams of Cloud Pak for Integration.

⚠️IMPORTANT: Please be noted. In this release, the focus is only on key/essential cabilities like PlatformNavigator, IBM APIConnect, IBM APPConnect, IBM Event Streams & IBM MQ. In future release, other capabilities like IBM Aspera, IBM DataPower, AssetRepo & OperationDashboard will be considered based on the customer or partner needs. ⚠️

Releases
Target Infrastructure
The Cloud Pak for Integration - automation assumes you have an OpenShift cluster already configured on your cloud of choice. The supported OpenShift options are ROSA for AWS, ROKS for IBM Cloud or OpenShift IPI instances.

Before you start to install and configure Cloud Pak for Integration, you will need to identify what your target infrastructure is going to be. You can start from scratch and use one of the pre-defined reference architectures from IBM or bring your own.

Reference Architectures
The reference architectures are provided in three different forms, with increasing security and associated sophistication to support production configuration. These three forms are as follows:

Quick Start - a simple architecture to quickly get an OpenShift cluster provisioned
Standard - a standard production deployment environment with typical security protections such as private endpoints, VPN server, key management encryption, etc
Advanced - a more advanced deployment that employs multiple virtual networks to isolate and securely route traffic between the different layers
For each of these reference architecture, we have provided a detailed set of automation to create the environment for the software. If you do not have an OpenShift environment provisioned, please use one of these. They are optimized for the installation of this solution.

Note: Cloud Pak for Integration system requirements recommend at least 3 worker nodes, with minimum 16vCPU per node and minimum 64 GB RAM per done (128 GB RAM is recommended).

Cloud Platform	Automation and Documentation
IBM Cloud	IBM Cloud Quick Start
IBM Cloud Standard - Coming soon
IBM Cloud Advanced - Coming soon
AWS	AWS Quick Start - Coming soon
AWS Standard - Coming soon
AWS Advanced - Coming soon
Azure	Azure Quick Start - Coming soon
Azure Standard - Coming soon
Azure Advanced - Coming soon
Bring Your Own Infrastructure	You will need a cluster with at least 16 CPUs and 64 GB of memory per node and at least 3 nodes to support storage and IBM Cloud Paks.
Getting Started
Within this repository you will find a set of Terraform template bundles that embody best practices for provisioning the Cloud Pak for Integration in multiple cloud environments. This README.md describes the SRE steps required to provision the CP4I software.

This suite of automation can be used for a Proof of Technology environment, or used as a foundation for production workloads with a fully working end-to-end cloud-native environment. The software installs using GitOps best practices with Red Hat Open Shift GitOps

Integration Architecture
The following reference architecture represents the logical view of how CP4I works after it is installed. CP4i is deployed with either Portworx or OpenShift Data Foundation storage, within an OpenShift Cluster, on the Cloud provider of your choice.

Reference Architecture

Deploying Cloud Pak for Integration
The following instructions will help you install Cloud Pak for Integration into AWS, Azure, and IBM Cloud OpenShift Kubernetes environment.

Licenses and Entitlements
Details on Cloud Pak for Integration licensing available at https://www.ibm.com/docs/en/cloud-paks/cp-integration/2021.4?topic=planning-licensing

Obtaining your IBM entitlement API key
To install Cloud Pak for Integration you are required to have an entitlement key that provides access to the software components. After the necessary entitlements have been granted, use the following steps to download the entitlement key and apply it to the automation:

Visit the Container Software Library site - https://myibm.ibm.com/products-services/containerlibrary
Log in with your IBMId credentials
Assuming the entitlements are in place, you will be presented with an entitlement key. Click "Copy key".
Open your "credentials.template" file and paste the entitlement key in for the value of TF_VAR_entitlement_key
Cloud Pak for Integration for Multi Cloud
The Integration capabilities automation is broken into what we call layers of automation or bundles. The bundles enable SRE activities to be optimized. The automation is generic between clouds other than configuration storage options, which are platform specific.

BOM ID	Name	Description	Runtime
200	200 - OpenShift Gitops	Set up OpenShift GitOps tools in an OpenShift cluster. This is required to install the software using gitops approaches.	10Mins
210	210 - IBM Portworx Storage
210 - IBM OpenShift Data Foundation
210 - AWS Portworx Storage
210 - Azure Portworx Storage	Use this automation to deploy a storage solution for your cluster.⚠️Portworks on AWS and Azure are currently not released, but are coming soon.⚠️	10 Mins
215	215 - Integration Platform Navigator	(MustRequire) Install integration Platform Navigator Operator & Instance into the cluster	45Mins
220	220 - Integration API Connect	(Optional) Install API Connect Operator & Instance into the cluster	30Mins
230	230 - Integration MQ	(Optional) Install IBM MQ Operator & Instance into the cluster	10Mins
240	240 - Integration App Connect Enterprise	(Optional) Install IBM ACE Operator & ACE Designer Instance into the cluster	20 Mins
250	250 - Integration EventStreams	(Optional) Install IBM EventStreams Operator & Instance into the cluster	10 Mins
260	260 - Integration MQ Uniform Cluster	(Optional) Install IBM MQ Uniform Cluster Instance into the cluster	10 Mins
280	280 - Cloud Pak for Integration	Deploy the Cloud Pak for Integration components (Platform Navigator/APIConnect/ACE/Event Streams/MQ
Pls Note:
⚠️If you choose to go with BOM ID: 280 then please ignore BOM ID 215/220/230/240/250/260 because 280 contains all the components of Cloud Pak for Integration ⚠️	90 to 100 Mins
Installation Steps
At this time the most reliable way of running this automation is with Terraform in your local machine either through a bootstrapped container image or with native tools installed. We provide a Container image that has all the common SRE tools installed. CLI Tools Image, Source Code for CLI Tools Before you start the installation please install the pre-req tools on your machine.

Before you start the installation please install the pre-req tools on your machine.

Pre-Req Setup
Please install the following Pre-Req tools to help you get started with the SRE tasks for installing Cloud Pak for Integration into an existing OpenShift Cluster on AWS, Azure, or IBM Cloud.

MacOS environment
Pre-requisites:

Check you have a valid GitHub ID that can be used to create a repository in your own organization GitHub or GitHub Enterprise account.
Install a code editor. We recommend Visual Studio Code
Install Brew
Ensure the following before continuing

Github account exists
A Github token is available with permissions set to create and remove repositories
You are able to login to the OpenShift cluster and obtain an OpenShift login token
Cloud Pak entitlement key, this can be obtained from visiting the IBM Container Library
Installing Integration Platform
The installation process will use a standard GitOps repository that has been built using the Modules to support CP4I platform installation. The automation is consistent across three cloud environments AWS, Azure, and IBM Cloud.

Set up the runtime environment
At this time the most reliable way of running this automation is with Terraform in your local machine either through a bootstrapped docker image or Virtual Machine. We provide both a container image and a virtual machine cloud-init script that have all the common SRE tools installed.

We recommend using Docker Desktop if choosing the container image method, and Multipass if choosing the virtual machine method. Detailed instructions for downloading and configuring both Docker Desktop and Multipass can be found in RUNTIMES.md

Set up environment credentials
First step is to clone the automation code to your local machine. Run this git command in your favorite command line shell.

git clone git@github.com:IBM/automation-integration-platform.git
From the command line, navigate into the automation-integration-platform folder from the command line

a. The README.md has the comprehensive instructions on how to install this into other cloud environments than TechZone. This document focuses on getting it running in a TechZone requested environment.

cd automation-integration-platform
Next you will need to set-up your credentials.properties file. This will enable secure access to your cluster.

cp credentials.template credentials.properties
code credential.properties
# Add the values for the Credentials to access the IBM Cloud
# Instructions to access this information can be found in the README.MD
# This is a template file and the ./launch.sh script looks for a file based on this template named credentials.properties

# The host for the git repository (e.g. github.com, bitbucket.org). Supported Git servers are GitHub, Github Enterprise, Gitlab, Bitbucket, Azure DevOps, and Gitea. If this value is left commented out, the automation will default to using Gitea.
#TF_VAR_gitops_repo_host=

## gitops_repo_username: The username of the user with access to the repository
#export TF_VAR_gitops_repo_username=

## gitops_repo_token: The personal access token used to access the repository
#export TF_VAR_gitops_repo_token=

# The organization/owner/group on the git server where the gitops repository will be provisioned/found. If not provided the org will default to the username.
#TF_VAR_gitops_repo_org=

# The project on the Azure DevOps server where the gitops repository will be provisioned/found. This value is only required for repositories on Azure DevOps.
#TF_VAR_gitops_repo_project

## TF_VAR_cluster_login_token: Token used for authentication to the api server
export TF_VAR_cluster_login_token=

## TF_VAR_server_url: The url for the OpenShift api server
export TF_VAR_server_url=

## TF_VAR_entitlement_key: The entitlement key used to access the IBM software images in the container registry. Visit https://myibm.ibm.com/products-services/containerlibrary to get the key
export TF_VAR_entitlement_key=


## TF_VAR_ibmcloud_api_key: IBM Cloud API Key required to provision storage on IBM Cloud
export TF_VAR_ibmcloud_api_key=

# Only needed if targeting AWS Deployment
export TF_VAR_access_key=
export TF_VAR_secret_key=

##
## Azure credentials
## Credentials are required to install Portworx on an Azure account. These credentials must have
## particular permissions in order to interact with the account and the OpenShift cluster. Use the
## provided `azure-portworx-credentials.sh` script to retrieve/generate these credentials.
##
# Only needed if targeting Azure Deployment

## TF_VAR_azure_subscription_id: The subscription id for the Azure account. This is required if Azure portworx is used
export TF_VAR_azure_subscription_id=
## TF_VAR_azure_tenant_id: The tenant id for the Azure account. This is required if Azure portworx is used
export TF_VAR_azure_tenant_id=
## TF_VAR_azure_client_id: The client id of the user for the Azure account. This is required if Azure portworx is used
export TF_VAR_azure_client_id=
## TF_VAR_azure_client_secret: The client id of the user for the Azure account. This is required if Azure portworx is used
export TF_VAR_azure_client_secret=
If you would like to use GitHub for your GitOps repo, then you will need to populate these values. Add your Git Hub username and your Personal Access Token to TF_VAR_gitops_repo_username and TF_VAR_gitops_repo_token. If these values are left blank, the automation will deploy Gitea into the OpenShift cluster for the GitOps deployment, requiring no additional user interaction.

From you OpenShift console click on top right menu and select Copy login command and click on Display Token

Copy Login Command

Copy the API Token value into the cluster_login_token value and Server URL into the server_url value, only the part starting with https

Copy Server URL and LoginToken

Copy the entitlement key, this can be obtained from visiting the IBM Container Library and place it in the entitlement_key variable.

Configure Storage
Deploying RookNFS Server on IBM Cloud, Azure & AWS
In Cloud Pak for Integration v2022.2.1, IBM introduced the support for deploying Rook NFS server which would facilitate the deployment of PlatformNavigator using the underlying RWO (ReadWriteOnce) storage. The instruction provided in https://www.ibm.com/docs/en/cloud-paks/cp-integration/2022.2?topic=ui-deploying-platform-rwo-storage is automated in this integration-automation asset. ⚠️IMPORTANT: Please be noted. Use this storage in case of PoC/PoTs/Demos. Leverage IBM ODF or Portworx for real customer engagement. ⚠️ If you decided to use this, move on to Set up the Runtime Environment section.

Deploying on IBM Cloud (Portworx or ODF)
Provide the IBM Cloud API Key for the target IBM Cloud account as the value for TF_VAR_ibmcloud_api_key
Deploying on Azure (Portworx)
If Cloud Pak for Integration will be deployed on OpenShift deployed on Azure, the credentials for the Azure account need to be provided. Several clis are required for these steps:

az cli - https://docs.microsoft.com/en-us/cli/azure/install-azure-cli
jq cli - https://stedolan.github.io/jq/download/
You can install these clis on your local machine OR run the following commands within the provided container image by running launch.sh

Log into your Azure account

az login
Run the azure-portworx-credentials.sh script to gather/create the credentials:

./azure-portworx-credentials.sh -t {cluster type} -g {resource group name} -n {cluster name} [-s {subscription id}]
where:

cluster type is the type of OpenShift cluster (aro or ipi).
resource group name is the name of the Azure resource group where the cluster has been provisioned.
cluster name is the name of the OpenShift cluster.
subscription id is the subscription id of the Azure account. If a value is not provided it will be looked up.
Update credentials.properties with the values output from the script.

{
  "azure_client_id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "azure_client_secret": "XXXXXXX",
  "azure_tenant_id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "azure_subscription_id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
If you used the container image to run the script, type exit to close the container shell then re-run launch.sh to pick up the changes to the environment variables.

Configure the automation
Get the Portworx configuration spec (for AWS or Azure deployments)
Follow the steps to download the portworx confituration spec
Copy the downloaded file into the root directory of the cloned automation-integration repository
Set up the runtime environment
At this time the most reliable way of running this automation is with Terraform in your local machine either through a bootstrapped docker image or Virtual Machine. We provide both a container image and a virtual machine cloud-init script that have all the common SRE tools installed.

We recommend using Docker Desktop if choosing the container image method, and Multipass if choosing the virtual machine method. Detailed instructions for downloading and configuring both Docker Desktop and Multipass can be found in RUNTIMES.md

⚠️IMPORTANT NOTE: ⚠️ DO-NOT SKIP RUNTIMES.md. In this step you need to setup either Docker Desktop or Multipass runtime. Without setting this up the further steps can not be executed.

We expect partners and clients will use their own specific Continuous Integration tools to support this the IBM team has focused on getting it installed in the least complicated way possible

Set up the automation workspace
Launch the automation runtime. Ensure the current working directory is 'automation-integration-platform'

If using Docker Desktop, run ./launch.sh. This will start a container image with the prompt opened in the /terraform directory.
If using Multipass, run mutlipass shell cli-tools to start the interactive shell, and cd into the /automation/{template} directory, where {template} is the folder you've cloned this repo. Be sure to run source credentials.properties once in the shell.
Next we need to set up the working directory for the automation: ⚠️IMPORTANT NOTE: ⚠️ setup-workspace.sh script is responsible for choosing the required module to be deployed on Openshift Cluster. The module we refer here is cater to "GitOps,Storage & Cloud Pak capabilities(PlatformNavigator,APIC,MQ,ACE & EventStreams)".

2a. If you decision was to use RookNFS storage

     ./setup-workspace-with-rook-NFS.sh [-p {cloud provider}] [-n {prefix name}] 
where:

cloud provider - the target cloud provider for the deployment (aws, azure, or ibm)
prefix name (optional) - the name prefix that will be used for the gitops repo
At this stage, We assume you have Openshift Cluster is up & running. Following info will help the user in setting up the right workspace.


    if [OpenShift Cluster is Provisoned on IBM Cloud ]
        ./setup-workspace-with-rook-NFS.sh -p ibm 
                
    if [OpenShift Cluster is Provisoned on AWS ]
        ./setup-workspace-with-rook-NFS.sh -p aws 
                
    if [OpenShift Cluster is Provisoned on Azure ]
        ./setup-workspace-with-rook-NFS.sh -p azure 
     
2b. If you decided to go with 'odf' or 'portworx' storage

     ./setup-workspace-with-odf-or-portworx.sh [-p {cloud provider}] [-s {storage}] [-r {region}] [-x {portworx spec file}] [-n {prefix name}] 
where:

cloud provider - the target cloud provider for the deployment (aws, azure, or ibm)
storage - the intended storage provider (portworx or odf)
region (mandatory in case of azure & aws) - the region where OpenShift Cluster is deployed
prefix name (optional) - the name prefix that will be used for the gitops repo
portworx spec file (mandatory in case of azure & aws) - the name of the file containing the Portworx configuration spec yaml
At this stage, We assume you have Openshift Cluster is up & running. Following info will help the user in setting up the right workspace.


    if [OpenShift Cluster is Provisoned on IBM Cloud && No Storage Provisioned]
        In IBM Cloud you have a choice to provision 'Open Data Foundation' or 'Portworx'
        In case 
            'OpenData Foundation' :
                ./setup-workspace-with-odf-or-portworx.sh -p ibm -s odf 
            'portworx' :
                ./setup-workspace-with-odf-or-portworx.sh -p ibm -s portworx -r [{ region where OCP is provisioned }] [-x {portworx spec file}]
                
    if [OpenShift Cluster is Provisoned on Azure && No Storage Provisioned]
        In Azure, at this point of time you can only go with 'portworx'
                ./setup-workspace-with-odf-or-portworx.sh -p azure -s portworx -r [{ region where OCP is provisioned }] [-x {portworx spec file}]

    if [OpenShift Cluster is Provisoned on aws && No Storage Provisioned]
        In aws, at this point of time you can only go with 'portworx'
                ./setup-workspace-with-odf-or-portworx.sh -p aws -s portworx -r [{ region where OCP is provisioned }] [-x {portworx spec file}]

     
Note: you can run the setup-workspace scripts without user prompting by using the -l command line argument as a comma separated list to specify the explicit layers by number.

For example, use layer 280 to specify all layers:

./setup-workspace-with-odf-or-portworx.sh -p ibm -s odf -n integration -l 280
Or, use individual layer numbers to only deploy those components.

./setup-workspace-with-odf-or-portworx.sh -p ibm -s odf -n integration -l 220,230,240
Layer numbers that can be used are:

280 All layers/components (this will override all other layers)
220 IBM API Connect
230 IBM MQ
240 IBM App Connect
250 IBM Event Streams
260 IBM MQ Uniform Cluster
The setup-workspace-odf-or-portworx.sh and setup-workspace-with-rook-NFS.sh scripts configure the cluster.tfvars and gitops.tfvars files with reasonable defaults.

Note: The default cluster.tfvars and gitops.tfvars files are symbolically linked to the new workspaces/current folder so this enables you to edit the file in your native operating system using your editor of choice.

The following are variables that you will be updating. Here are some suggested values.

Variable	Description	Suggested Value
rwx_storage_class	The storage class to use for Read-Write-Many volumes.	In case of 'ibm cloud' & 'odf' storage it is 'ocs-storagecluster-cephfs'. In case of 'aws' cloud & 'portworx' storage it is 'portworx-gp3-sc'. In case of 'azure' cloud & 'portworx' storage it is 'portworx-gp3-sc'. In case of 'aws' or 'aws' or 'azure' cloud & 'RookNFS' storage it is 'integration-storage'
rwo_storage_class	The storage class to use for Read-Write-One volumes. on aws: gp2, on azure: managed-premium, on ibm: ibmc-vpc-block-10iops-tier	In case of 'ibm cloud' & 'odf' storage it is 'ibmc-vpc-block-10iops-tie'. In case of 'aws' cloud & 'portworx' storage it is 'gp2'. In case of 'azure' cloud & 'portworx' storage it is 'managed-premium'
gitops_repo_host	The host for the git repository.	github.com
gitops_repo_type	The type of the hosted git repository (github or gitlab).	github
gitops_repo_org	The org/group where the git repository will be created. If the value is left blank then it will default to the username	github userid or org
gitops_repo_repo	The short name of the repository to create	cp4i-gitops
config_banner_text	Banner text for the cluster console	Cloud Pak for Integration
portworx_spec	The content of Portworx yaml in base64 encoded format	Will be setup by setup-workspace-with-odf-or-portworx
Scan through the terraform.tfvars file thouroughly and double check the values based on your environment.
Save the terraform.tfvars file.
Apply the automation
We are now ready to start installing Cloud Pak for Integration. Ensure you are inside the running container or Multipass VM.

Within the container terminal, change directory to the /workspaces/current folder. This folder was populated by the setup-workspaces.sh script in the previous step. (The launch.sh command configures a named volume to preserve the contents of the /workspaces directory between container images so you don't need to re-run setup-workspaces.sh again unless you want to configure a different environment.)

Run ./apply-all.sh to kick off the automation. The script will apply each layer in order. Cloud Pak fo Integration deployment will run asynchronously in the background, and may require up to 90 to 100 minutes to complete.

You can check the progress by looking at two places, first look in your github repository. You will see the git repository has been created based on the name you have provided. The Cloud Pak for Integration install will populate this with information to let OpenShift GitOps install the software. The second place is to look at the OpenShift console, Click Workloads->Pods and you will see the GitOps operator being installed. You can also check the progress of the deployment by opening up Argo CD (OpenShift GitOps). From the OpenShift user interface, click on the Application menu 3x3 Icon on the header and select Cluster Argo CD menu item.)

Once deployment is complete, go back into the OpenShift cluster user interface and navigate to view Routes for the cp4i-pn namespace. Here you can see the URL to the deployed Platform Navigator instance. Open this url in a new browser window.

Route

Navigate to Secrets in the ibm-common-services namespace, and find the platform-auth-idp-credentials secret. Copy the value of password key inside of that secret.

Go back to the PlatformNavigator instance that you opened in a separate window. Log in using the username admin with the password copied in the previous step.

Summary
This concludes the instructions for installing *Cloud Pak for Integration * on AWS, Azure, and IBM Cloud

Troubleshooting
Please refer to the Troubleshooting Guide for uninstallation instructions and instructions to correct common issues.

How to Generate this repository from the source Bill of Materials.
This set of automation packages was generated using the open-source isacable tool. This tool enables a Bill of Material yaml file to describe your software requirements. If you want up stream releases or versions you can use iascable to generate a new terraform module.

The iascable tool is targeted for use by advanced SRE developers. It requires deep knowledge of how the modules plug together into a customized architecture. This repository is a fully tested output from that tool. This makes it ready to consume for projects.
