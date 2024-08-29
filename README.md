# CAST AI Technical Workshop
#### This is work in progress

This is a quickstart guide to get your clusters up and running for the labs.


## AWS
#### Pre Requisites
- AWS Account with administrative priviledges
- AWS CLI installed
- EKSCTL CLI Installed
- jq
- Kubectl
- Helm


Authenticate in your AWS Account to use the CLI

```
aws configure
```

Create an EKS Cluster with 6 nodes
```
eksctl create cluster --name eks-lab-cast --region us-east-2 --version 1.28 -N 6 --vpc-nat-mode Disable;
```

Now that you have your cluster created, run the command below to update your local kubeconfig.

```
aws eks update-kubeconfig --name eks-lab-cast --region us-east-2
```

Install Metrics Server if it's not already installed.
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/high-availability-1.21+.yaml
```

If you want to restore the original nodes from the EKS Nodegroup, to re-run rebalancer or evictor, you can scale up your cluster with this command.
```
aws eks update-nodegroup-config --region us-east-2 --cluster-name eks-lab-cast --nodegroup-name ng-753705db --scaling-config desiredSize=6
```

Cleaning up. If you don't want to keep your cluster after the labs, use the command below to delete it.
```
eksctl delete cluster --region=us-east-2 --name=eks-lab-cast
```


## Azure
#### Pre Requisites
- Azure Account with administrative priviledges
- Azure CLI (az)
- jq
- Kubectl
- Helm

 ```
 az login
```

Set subscription you would like to deploy do using command: 
```
az account set --subscription (subscriptionID)
```

Verify Microsoft.OperationsManagement and Microsoft.OperationalInsights providers are registered on your subscription. 
```
az provider show -n Microsoft.OperationsManagement -o table
az provider show -n Microsoft.OperationalInsights -o table
```

Create a Resource Group for AKS to reside 
```
  az group create --name (rg-name) --location eastus2
```
Create an AKS Cluster
```
az aks create -g (rg-name) -n (aks-cluster-name) --location eastus2 --node-count 5 -s Standard_DS3_v2
```

Get access to your cluster
```
az aks get-credentials --resource-group (rg-name)--name (aks-cluster-name)
```

Test acccess
```
kubectl get nodes
```

Cleaning up: If you don't need your cluster anymore after the labs, use the command below to delete it.
```
az aks delete --name (aks-cluster-name) --resource-group (rg-name) --yes --no-wait
az group delete --resource-group (rg-name) --yes --no-wait
```

## GCP
#### Pre Requisites
- GCP Account with administrative priviledges
- GCloud CLI 
- jq
- Kubectl
- Helm

Initializing the CLI: https://cloud.google.com/sdk/docs/initializing

Creating your cluster
 ```
 gcloud container clusters create <gke-cluster-name> --num-nodes=4
 ```

Testing Access
```
kubectl get nodes
```

Cleaning Up: If you don't need your cluster after the Labs, execute the command below to delete it.
```
gcloud container clusters delete <cluster-name>
```
