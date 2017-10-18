# ACS-KubeWalkthrough
The following provides a basic walkthrough of deploying a Kubernetes cluster on Azure Container Service, and deploying your first application on the cluster. A longer version can be found at the following:

https://docs.microsoft.com/en-us/azure/container-service/kubernetes/container-service-kubernetes-walkthrough

For the purposes of this walk through I'll run in the Azure Cloud Shell and SSH into the master node to run kubectl. This is an ok approach for a quick walkthrough that you dont intent to keep running longer than the length of the lab, but if you intent to step away you should run from the Azure CLI on a dedicated host, like your own laptop, instead.

1. Connect to the Azure Portal and open the cloud shell
![Cloud Shell](/images/cloudshell.PNG)


![Cloud Shell](/images/cloudshell2.PNG)

2. If you have more than one subscription you can set the subscription by running the following:

```
az account list
#Get the subscription ID
az account set -s <subscriptionID>
```

3. Create a new resource group for the walkthough

```
RG=K8sWalkthrough
LOC=northeurope
CLUSTERNAME=K8sWalkthrough
az group create --resource-group $RG --location $LOC

az acs create --orchestrator-type Kubernetes --name $CLUSTERNAME --resource-group $RG --location $LOC --admin-username <yourUserName> --admin-password <yourPassword)
```

Note: In the above I use user name and password options. If you prefer to use an ssh key instead you can leave those out and use the --generate-ssh-keys flag. That will generate the necessary keys in your .ssh folder. Just keep in mind that if you're using the cloud shell the .ssh folder will not persist if you lose your session.

