

# Oracle Container Engine for Kubernetes (OKE) Cluster Creation Guide - Quick Create Managed Cluster

# Prerequisites

Before proceeding, ensure you have:
- An active Oracle Cloud Infrastructure (OCI) account
- Administrator privileges in your tenancy
- Sufficient quota allocation
- Appropriate IAM policies configured in your target compartment
- Access to OCI Cloud Shell

# Quick Create Cluster Overview

The Quick Create option provides a streamlined setup process with the following characteristics:

# Default Settings
- VCN CIDR: Fixed at 10.0.0.0/16 (not modifiable in Quick Create)
- Node architecture: AMD CPU
- Access method: OCI Cloud Shell

# Important Considerations

# Node Pool Image Selection
When configuring your node pool, you'll encounter two image options:
1. OCI Platform Image
2. OCI Kubernetes Image

**Critical Note**: Pay special attention when selecting OKE images, as they don't explicitly display CPU architecture information. Selecting an incompatible image architecture will cause node pool creation to fail.

Despite having identical names, these images are built for different architectures.

# Resource Cleanup

After testing your cluster, remember:
1. Deleting the cluster will not automatically remove:
   - Persistent volumes created by OKE
   - The associated VCN
2. The VCN must be manually deleted after cluster deletion

# Best Practices
- Document your configuration choices
- Test cluster access immediately after creation
- Verify node pool connectivity
- Monitor resource usage during initial deployment

# Target Audience
This guide assumes:
- Basic familiarity with Linux commands
- Understanding of Kubernetes concepts
- Experience with OCI console navigation

# Support Resources
If you encounter issues:
- Check OCI documentation
- Review OKE troubleshooting guides
- Contact OCI support for architecture-specific questions

---
*Note: This guide focuses on command-line operations and essential configurations. For production deployments, additional security and networking considerations should be evaluated.*




# Test sample deployment of nginx

kubectl create deployment nginx --image=nginx:latest --port=80 --replicas=3
kubectl get pods

# Inspect the pods
kubectl describe pod <pod-name>


kubectl expose deployment nginx --port=80 --type=LoadBalancer
kubectl get services

# Delete the deployment & its service
kubectl delete deployment nginx 
kubectl delete service nginx



You'll need to replace all placeholder values (<...>) with actual values from your OCI environment:

compartment_ocid
vcn_ocid
subnet_ocids
availability_domain
region


The script creates:

A new VCN with public and private subnets
A managed Kubernetes cluster
A node pool with 3 nodes








# 1. Install OCI CLI (if not already installed)
curl -L -O https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh
bash install.sh --accept-all-defaults

# 2. Configure OCI CLI
oci setup config

# 3. Install kubectl (if not already installed)
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

# 4. Create a new compartment (optional)
oci iam compartment create \
    --compartment-id <parent_compartment_ocid> \
    --name "oke-compartment" \
    --description "Compartment for OKE cluster"

# 5. Create Virtual Cloud Network (VCN)
oci network vcn create \
    --compartment-id <compartment_ocid> \
    --display-name "oke-vcn" \
    --cidr-block "10.0.0.0/16"

# 6. Create subnets
# Create public subnet for load balancers
oci network subnet create \
    --compartment-id <compartment_ocid> \
    --vcn-id <vcn_ocid> \
    --display-name "oke-public-subnet" \
    --cidr-block "10.0.1.0/24"

# Create private subnet for worker nodes
oci network subnet create \
    --compartment-id <compartment_ocid> \
    --vcn-id <vcn_ocid> \
    --display-name "oke-private-subnet" \
    --cidr-block "10.0.2.0/24"

# 7. Create OKE cluster
oci ce cluster create \
    --compartment-id <compartment_ocid> \
    --name "my-oke-cluster" \
    --vcn-id <vcn_ocid> \
    --kubernetes-version "v1.24.1" \
    --service-lb-subnet-ids '["<public_subnet_ocid>"]' \
    --subnet-ids '["<private_subnet_ocid>"]' \
    --node-pools '[{
        "name": "pool1",
        "node-shape": "VM.Standard.E4.Flex",
        "node-shape-config": {
            "ocpus": 2,
            "memory-in-gbs": 16
        },
        "node-config-details": {
            "size": 3,
            "placement-configs": [{
                "availability-domain": "<availability_domain>",
                "subnet-id": "<private_subnet_ocid>"
            }]
        }
    }]'

# 8. Get cluster kubeconfig
oci ce cluster create-kubeconfig \
    --cluster-id <cluster_ocid> \
    --file $HOME/.kube/config \
    --region <region>

# 9. Verify cluster access
kubectl get nodes

# 10. (Optional) Scale node pool
oci ce node-pool update \
    --node-pool-id <node_pool_ocid> \
    --size 5

# 11. (Optional) List available k8s versions
oci ce cluster-options get \
    --cluster-option-id all

# Notes:
# - Replace all <placeholders> with actual values
# - Ensure necessary IAM policies are in place
# - Configure security lists appropriately
# - Consider enabling cluster autoscaler if needed
# - Monitor cluster metrics using OCI monitoring





# Test sample deployment of nginx

kubectl create deployment nginx --image=nginx:latest --port=80 --replicas=3
kubectl get pods

# Inspect the pods
kubectl describe pod <pod-name>


kubectl expose deployment nginx --port=80 --type=LoadBalancer
kubectl get services

# Delete the deployment & its service
kubectl delete deployment nginx 
kubectl delete service nginx


