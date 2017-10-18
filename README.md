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

4. After the process comletes, usually 5-10 minutes, you can ssh into the master node as follows:

```
ssh <username>@<masterFQDN>
```
Note: You can get the master FQDN from the output of the deployment, or you can get it from the portal by navigating to your resource group and then 

![Cloud Shell](/images/ssh.PNG)

You can alternatively install kubectl locally and get the credentials to access your cluster with the following commands.

```
az acs kubernetes install-cli
az acs kubernetes get-credentials --resource-group $RG --name $CLUSTERNAME
```

5. Explore your cluster with a few commands
```
kubectl cluster-info
kubectl get nodes
```
Note: You may get proto errors when you run commands on the master node, but this shouldnt stop you from completing the walkthrough.  

6. Deploy your first container
```
kubectl run nginx --image nginx
kubectl get pods
kubectl get rs
```
You now have a pod and a replica set runnning in your cluster, but it is not exposed publically.

7. Use the expose command to expose your nginx container. You can then watch the service creation to see the Azure public IP eventually assigned.

```
kubectl expose deployment nginx --type=LoadBalancer --port=80

kubectl get svc --watch
```
Note: --type=LoadBalancer is how Kubernetes knows to call Azure and create the external IP and loadbalancer rules to expose your pod.

8. Once the External IP is assigned, you can copy that and navigate to that URL in your browser. You should see your nginx pod up and running


If you'd like to try out the Azure Container Instance connector you can follow instructions from the link below:
https://github.com/Azure/aci-connector-k8s/blob/master/README.md

