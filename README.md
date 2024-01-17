# Automate Azure Kubernetes Service (AKS) Cluster Creation and App Deployment with Terraform and Kubectl

## Overview
This project provides a powerful yet easy-to-use infrastructure as code (IaC) solution for automating the creation of an AKS cluster using Terraform. It also facilitates app deployment on the managed AKS cluster using Kubectl. The main goal of this project is to save you time and reduce manual error by automating complex processes in a repeatable and reliable manner.

## Prerequisites
Ensure you have installed and correctly configured the following tools:

- VSCode
- Terraform
- Azure CLI
- kubectl

## Getting Started

### Setup & Installation
1. Clone this repository using git:
```bash 
git clone https://github.com/johnkdevops/aks-cluster-create-terraform-project.git
```

2. Navigate to the directory of the cloned repository's directory. run:
```bash
cd aks-cluster-create-terraform-project
```

## Deployment

### Authenticate & Connect to Azure
Execute the following command and follow the instructions:
```bash
az login 
```

### Initialize Terraform
Ensure Terraform is initialised in your project directory:
```bash
terraform init -upgrade
```

### Review Terraform Plan
Review the changes Terraform will perform with the following command:
```bash
terraform plan -out main.tfplan
```

### Create the AKS Cluster
To apply the Terraform configuration, run:
```bash
terraform apply --auto-approve
```

### Prep the Kubeconfig File
To copy your kubeconfig file to your home directory, execute:
```bash
echo "$(terraform output kube_config)" > ~/azurek8s
```

### Connect to the AKS Cluster
Execute the following command and follow the instructions:
```bash
az login 
```
Second, set the correct Azure subscription:
```bash
AZSUB="$(terraform output subscription_id)"
az account set --subscription $AZSUB
```
Third, execute this command to connect to the AKS cluster:
```bash
AKSRG="$(terraform output resource_group_name)" 
AKSNAME="$(terraform output azurerm_kubernetes_cluster_name)" 
az aks get-credentials -g $AKSRG -n $AKSNAME --overwrite-existing
```
Verify your AKS Nodes are ready by running:
```bash
kubectl get nodes
```

### 6. Deploy the Application
To deploy the application onto the AKS cluster, run:
```bash
kubectl apply -f .
```

Verify your application was deployed and service is added:
```bash
kubectl get deploy,po,svc
```

### 7. Test the Application
To access your deployed application, use the External IP Address provided by the service:
```bash
kubectl get svc -o wide
```

Copy the External IP address of the service, paste it into your browser, and you should see the application landing page!

## 8. Cleaning up

To prevent any unwanted costs and tidy up your environment, delete the resources:

```bash
kubectl delete --force --grace-period=0 service/swiggy-app; kubectl delete--force --grace-period=0 deployment.apps/swiggy-app
```

Verify kubernetes resources were successfully deleted:
```bash
kubectl get deploy,svc
```

Delete AKS resources run:
```bash
terraform destroy --auto-approve
```

## Acknowledgements
I want to thank Ashfaque Ahmed Shaikh for great project.

## Contributing
Contributions are welcome! Please fork this repository and open a pull request to add more functionalities.

## License
[MIT](https://choosealicense.com/licenses/mit/)
