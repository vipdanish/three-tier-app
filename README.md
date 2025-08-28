
# Three-Tier Web Application
![Last Commit](https://img.shields.io/github/last-commit/vipdanish/three-tier-app)
![License](https://img.shields.io/github/license/vipdanish/three-tier-app)
![React](https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black)
![Node.js](https://img.shields.io/badge/Node.js-339933?logo=node.js&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?logo=mongodb&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-FF9900?logo=amazon-aws&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white)
![EC2](https://img.shields.io/badge/EC2-FF9900?logo=amazon-aws&logoColor=white)
![EKS](https://img.shields.io/badge/EKS-FF9900?logo=amazon-aws&logoColor=white)
![ECR](https://img.shields.io/badge/ECR-FF9900?logo=amazon-aws&logoColor=white)
![IAM](https://img.shields.io/badge/IAM-FF9900?logo=amazon-aws&logoColor=white)
![Load Balancer](https://img.shields.io/badge/ALB/NLB-FF9900?logo=amazon-aws&logoColor=white)
![S3](https://img.shields.io/badge/S3-FF9900?logo=amazon-aws&logoColor=white)
![CloudWatch](https://img.shields.io/badge/CloudWatch-FF9900?logo=amazon-aws&logoColor=white)
![Route 53](https://img.shields.io/badge/Route53-FF9900?logo=amazon-aws&logoColor=white)



This repository hosts a **Three-Tier Web Application** built with **ReactJS**, **NodeJS**, and **MongoDB**, deployed on **AWS EKS**.  
The project demonstrates full-stack development, CI/CD pipelines, and cloud-native deployment practices. 🚀

---

## Demo / Screenshot



---

## Project Overview
This project includes:  
- **Frontend:** ReactJS  
- **Backend:** NodeJS + Express  
- **Database:** MongoDB  
- **CI/CD:** Jenkins pipeline  
- **Infrastructure:** AWS EKS, Terraform  
- **Monitoring:** Prometheus + Grafana  
- **GitOps:** ArgoCD  

The project demonstrates:  
- Full-stack development and deployment  
- Containerization with Docker  
- Kubernetes orchestration on AWS EKS  
- Automated CI/CD pipelines  
- Monitoring and GitOps best practices

---

## Project Structure
- **Application-Code**: Source code for frontend and backend implementations  
- **Jenkins-Pipeline-Code**: CI/CD scripts  
- **Terraform Scripts**: Infrastructure provisioning on AWS  
- **Kubernetes Manifests**: Deployment files for EKS  
<img width="5264" height="2994" alt="image" src="https://github.com/user-attachments/assets/e5b83636-f656-4455-b767-81b694cb0f75" />
---

## Getting Started

### Prerequisites
- Basic knowledge of Docker and AWS services  
- AWS account with necessary permissions

### Setup Steps

#### 1. IAM Configuration
- Create user `eks-admin` with `AdministratorAccess`.  
- Generate **Access Key** and **Secret Access Key**.

#### 2. EC2 Setup
- Launch an Ubuntu instance (e.g., `us-west-2`) and SSH into it.

#### 3. Install AWS CLI v2
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin --update
aws configure
````

#### 4. Install Docker

```bash
sudo apt-get update
sudo apt install docker.io
docker ps
sudo chown $USER /var/run/docker.sock
```

#### 5. Install kubectl

```bash
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```

#### 6. Install eksctl

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

#### 7. Setup EKS Cluster

```bash
eksctl create cluster --name three-tier-cluster --region us-west-2 --node-type t2.medium --nodes-min 2 --nodes-max 2
aws eks update-kubeconfig --region us-west-2 --name three-tier-cluster
kubectl get nodes
```

#### 8. Apply Kubernetes Manifests

```bash
kubectl create namespace workshop
kubectl apply -f .
kubectl delete -f .
```

#### 9. Install AWS Load Balancer Controller

```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json
eksctl utils associate-iam-oidc-provider --region=us-west-2 --cluster=three-tier-cluster --approve
eksctl create iamserviceaccount --cluster=three-tier-cluster --namespace=kube-system --name=aws-load-balancer-controller --role-name AmazonEKSLoadBalancerControllerRole --attach-policy-arn=arn:aws:iam::626072240565:policy/AWSLoadBalancerControllerIAMPolicy --approve --region=us-west-2
```

#### 10. Deploy AWS Load Balancer Controller

```bash
sudo snap install helm --classic
helm repo add eks https://aws.github.io/eks-charts
helm repo update eks
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=my-cluster --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller
kubectl get deployment -n kube-system aws-load-balancer-controller
kubectl apply -f full_stack_lb.yaml
```

### Cleanup

```bash
eksctl delete cluster --name three-tier-cluster --region us-west-2
# Stop/terminate EC2 and delete other resources to avoid charges
```

---

## Contribution Guidelines

* Fork the repository and create a feature branch
* Add enhancements or fixes
* Ensure code follows the project style
* Submit a Pull Request with a detailed description of changes

---

## Support

For any queries or issues, open an **issue** in this repository.

