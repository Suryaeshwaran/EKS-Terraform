# EKS-Terraform
Deploy an EKS cluster in the KodeKloud AWS Playground using Terraform.
This terraform code will create an EKS cluster called demo-eks.

**Clone This Repo**

```
git clone https://github.com/Suryaeshwaran/EKS-Terraform.git
```

**Create EKS Cluster Via Terraform**

```
terraform init
terraform plan
terraform apply -auto-approve
```
This may take up to 10-20 minutes to complete. When it completes, you will see something similar to this at the end of all the output. You will need the value of NodeInstanceRole later.
```
Outputs:

NodeAutoScalingGroup = "demo-eks-stack-NodeGroup-UUJRINMIFPLO"
NodeInstanceRole = "arn:aws:iam::AWS-ACCOUNTNUMBER:role/demo-eks-node"
NodeSecurityGroup = "sg-003010e8d8f9f32bd"
```
**Set up access and join nodes**
1. Create a KUBECONFIG for kubectl
```
aws eks update-kubeconfig --region us-east-1 --name demo-eks
```
2. Join the worker nodes
Edit the ConfigMap YAML to add in the NodeInstanceRole we got from terraform
```
vi aws-auth-cm.yaml
```
Delete the text <ARN of instance role (not instance profile)> and replace it with the value for NodeInstanceRole we got from terraform, then save and exit.

```
apiVersion: v1
kind: ConfigMap
metadata:
name: aws-auth
namespace: kube-system
data:
mapRoles: |
    - rolearn: <Replace NodeInstanceRole from Output> # <- EDIT THIS
    username: system:node:{{EC2PrivateDNSName}}
    groups:
        - system:bootstrappers
        - system:nodes
```
3. Apply the edited ConfigMap to join the nodes
```
kubectl apply -f aws-auth-cm.yaml
```
Wait 2-3 minutes for node join to complete, then
```
kubectl get node -o wide
```
You should see 3 worker nodes in ready state. Note that with EKS you do not see control plane nodes, as they are managed by AWS.

4. View the completed cluster in the EKS Console.
