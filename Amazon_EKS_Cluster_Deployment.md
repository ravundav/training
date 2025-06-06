# Amazon EKS Cluster Creation & Deployment Guide

This guide provides step-by-step instructions and essential commands for creating an Amazon EKS (Elastic Kubernetes Service) cluster and deploying applications.

---

## Prerequisites

- **AWS CLI** installed and configured (`aws configure`)
- **kubectl** installed
- **eksctl** installed

---

## 1. Create an EKS Cluster

```bash
eksctl create cluster \
  --name my-cluster \
  --region us-west-2 \
  --nodegroup-name my-nodes \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3 \
  --managed
```

---

## 2. Update kubeconfig

```bash
aws eks --region us-west-2 update-kubeconfig --name my-cluster
```

---

## 3. Verify Cluster

```bash
kubectl get nodes
kubectl get svc
```

---

## 4. Deploy an Application (Example: NGINX)

```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=LoadBalancer
kubectl get svc
```

---

## 5. Important EKS & kubectl Commands

### Cluster Management

- List clusters:  
  `eksctl get cluster`
- Delete cluster:  
  `eksctl delete cluster --name my-cluster --region us-west-2`

### Node Group Management

- List nodegroups:  
  `eksctl get nodegroup --cluster my-cluster`
- Add nodegroup:  
  `eksctl create nodegroup --cluster my-cluster --name extra-nodes --nodes 2`
- Delete nodegroup:  
  `eksctl delete nodegroup --cluster my-cluster --name extra-nodes`

### kubectl Basics

- Get pods:  
  `kubectl get pods`
- Get deployments:  
  `kubectl get deployments`
- Describe resource:  
  `kubectl describe pod <pod-name>`
- Apply manifest:  
  `kubectl apply -f <file.yaml>`
- Delete resource:  
  `kubectl delete -f <file.yaml>`

---

## 6. Clean Up

```bash
eksctl delete cluster --name my-cluster --region us-west-2
```

---

## References

- [EKS Documentation](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html)
- [eksctl Documentation](https://eksctl.io/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## Demo
```
EKS cluster creation:
Access from EC2:
Create IAM user and generate access key and secret key
Create EC2 -  install awscli and configure with secret key and access key(aws configure)
Install kubectl
Install eksctl
Create eks by using below command
eksctl create cluster --name eks-demo --region us-east-1 --nodegroup-name standard-workers --node-type t3.medium --nodes 3 --nodes-min 1 --nodes-max 3 --managed
Go to ec2 dash board and check worker nodes should be ready
Go to eks dash board and check
# eksctl get cluster (from ec2)
# aws eks update-kubconfig –name eks-demo –region us-east-1
# vi deployment.yml ( if required we can create deployment)
# kubectl apply -f deployment.yml

# aws cloudformation list-stacks \
  --stack-status-filter CREATE_COMPLETE \
  --query "StackSummaries[].{StackName:StackName,StackStatus:StackStatus}" \
  --output table

------------------------------------------------------------------------------
|                                 ListStacks                                 |
+--------------------------------------------------------+-------------------+
|                        StackName                       |    StackStatus    |
+--------------------------------------------------------+-------------------+
|  eksctl-eks-prasaad-nodegroup-standard-workers         |  CREATE_COMPLETE  |
|  eksctl-eks-naveen-latest-nodegroup-standard-workers   |  CREATE_COMPLETE  |
|  eksctl-eks-mani-nodegroup-standard-workers            |  CREATE_COMPLETE  |
|  eksctl-eks-sohitha-nodegroup-standard-workers         |  CREATE_COMPLETE  |
|  eksctl-eks-mrudula-cluster                            |  CREATE_COMPLETE  |
|  eksctl-madhavi-eks-nodegroup-madhavi-standard-workers |  CREATE_COMPLETE  |
|  eksctl-eks-demo-puja-cluster                          |  CREATE_COMPLETE  |
|  eksctl-eks-sandeepk-nodegroup-standard-workers        |  CREATE_COMPLETE  |
|  eksctl-eks-prasaad-cluster                            |  CREATE_COMPLETE  |
|  eksctl-charan-nodegroup-standard-workers              |  CREATE_COMPLETE  |
|  eksctl-eks-bhavani-nodegroup-standard-workers         |  CREATE_COMPLETE  |
|  eksctl-eks-demo-uday-nodegroup-standard-workers       |  CREATE_COMPLETE  |
|  eksctl-eks-sai-nodegroup-standard-workers-sai         |  CREATE_COMPLETE  |
|  eksctl-eks-demo-nikhil-nodegroup-standard-workers     |  CREATE_COMPLETE  |
|  eksctl-eks-naveen-latest-cluster                      |  CREATE_COMPLETE  |
|  eksctl-eks-sohitha-cluster                            |  CREATE_COMPLETE  |
|  eksctl-eks-mani-cluster                               |  CREATE_COMPLETE  |
|  eksctl-madhavi-eks-cluster                            |  CREATE_COMPLETE  |
|  eksctl-eks-sandeepk-cluster                           |  CREATE_COMPLETE  |
|  eksctl-charan-cluster                                 |  CREATE_COMPLETE  |
|  eksctl-eks-bhavani-cluster                            |  CREATE_COMPLETE  |
|  eksctl-eks-demo-uday-cluster                          |  CREATE_COMPLETE  |
|  eksctl-eks-sai-cluster                                |  CREATE_COMPLETE  |
|  eksctl-eks-nag-demo-nodegroup-standard-workers        |  CREATE_COMPLETE  |
|  eksctl-eks-demo-nikhil-cluster                        |  CREATE_COMPLETE  |
|  eksctl-eks-prasad-cluster                             |  CREATE_COMPLETE  |
|  eksctl-eks-demo-cluster                               |  CREATE_COMPLETE  |
|  eksctl-eks-nag-demo-cluster                           |  CREATE_COMPLETE  |
|  eksctl-rk-demo-nodegroup-standard-workers             |  CREATE_COMPLETE  |
|  eksctl-rk-demo-cluster                                |  CREATE_COMPLETE  |
+--------------------------------------------------------+-------------------+
```
