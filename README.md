# Learn Terraform - Provision an EKS Cluster

This repo is a companion repo to the [Provision an EKS Cluster learn guide](https://learn.hashicorp.com/terraform/kubernetes/provision-eks-cluster), containing
Terraform configuration files to provision an EKS cluster on AWS.

## How to provision the cluster
These steps are taken from [Provision an EKS Cluster (AWS) tutorial by HashiCorp](https://learn.hashicorp.com/tutorials/terraform/eks)

1. Configure AWS credentials
```shell
aws configure
```

2. Initialize Terraform workspace
```shell
terraform init
```

3. Provision the EKS cluster
```shell
terraform apply
```

4. Configure `kubectl`
```shell
aws eks --region $(terraform output -raw region) update-kubeconfig \
    --name $(terraform output -raw cluster_name)
```

5. Verify that the Kubernetes control plane location matches the `cluster_endpoint` value from the terraform apply output:
```shell
kubectl cluster-info

Kubernetes control plane is running at https://D65F67DE09B79A42B3EC4AFF0DDCD855.gr7.us-east-1.eks.amazonaws.com
CoreDNS is running at https://D65F67DE09B79A42B3EC4AFF0DDCD855.gr7.us-east-1.eks.amazonaws.com/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

6. Verify that all three worker nodes are part of the cluster
```shell
kubectl get nodes

NAME                        STATUS   ROLES    AGE   VERSION
ip-10-0-1-93.ec2.internal   Ready    <none>   22m   v1.22.12-eks-ba74326
ip-10-0-2-51.ec2.internal   Ready    <none>   22m   v1.22.12-eks-ba74326
ip-10-0-2-9.ec2.internal    Ready    <none>   22m   v1.22.12-eks-ba74326
```

## How to clean up your workspace
```shell
terraform destroy
```