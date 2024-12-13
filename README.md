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

