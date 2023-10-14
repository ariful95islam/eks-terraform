# AWS EKS Terraform Setup with Nginx Deployment

This repository contains Terraform configurations for setting up an EKS cluster on AWS and deploying an Nginx service to it. It also has a remost state feature so that state can be shared with local and github workflow. 

## Structure

- `vpc.tf`: Contains the Terraform configuration for setting up a VPC in AWS using the `terraform-aws-modules/vpc/aws` module.
- `eks-cluster.tf`: Contains the Terraform configuration for setting up an EKS cluster in the VPC using the `terraform-aws-modules/eks/aws` module.
- `nginx.yaml`: Contains the Kubernetes deployment and service configurations for running Nginx in the EKS cluster.

## Prerequisites

Ensure that you have the following installed:

- Terraform
- AWS CLI
- `kubectl`
- `aws-iam-authenticator`

## Setup

1. **Create a `terraform.tfvars` file**:
   Populate the file with necessary variables. Here's an outline of how the file should look:

   ```hcl
   vpc_cidr_block = "your_vpc_cidr_block"
   private_subnet_cidr_blocks = ["subnet1_cidr", "subnet2_cidr", ...]
   public_subnet_cidr_blocks = ["subnet1_cidr", "subnet2_cidr", ...]
   # Add any other required variables here
   ```

   Replace placeholders like `your_vpc_cidr_block` and `subnet1_cidr` with appropriate values.

2. **Initialize Terraform**:
   ```bash
   terraform init
   ```

3. **Apply Terraform configurations**:
   ```bash
   terraform apply
   ```

4. **Connect to the EKS cluster**:
   Update your kubeconfig to point to the newly created EKS cluster:
   ```bash
   aws eks update-kubeconfig --name myapp-eks-cluster --region eu-west-2
   ```

5. **Deploy Nginx to EKS**:
   ```bash
   kubectl apply -f nginx.yaml
   ```

6. **Access Nginx**:
   Once the Nginx service is deployed, it will be exposed through a LoadBalancer. You can retrieve the LoadBalancer URL or IP using:
   ```bash
   kubectl get svc nginx
   ```

## Cleanup

1. **Delete Nginx Deployment**:
   ```bash
   kubectl delete -f nginx.yaml
   ```

2. **Destroy EKS and VPC**:
   ```bash
   terraform destroy
   ```

## Notes

- Ensure that you have sufficient permissions in AWS for all operations.
- Costs may be incurred when resources are provisioned in AWS.
- Always review and customise configurations as per your requirements.
