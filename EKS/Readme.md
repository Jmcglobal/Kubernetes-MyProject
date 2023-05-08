## EKS - Elastic Kubernetes Service

EKS (Elastic Kubernetes Service) is a managed Kubernetes service provided by Amazon Web Services (AWS). 
It allows users to quickly and easily create and manage Kubernetes clusters in the cloud. EKS is designed to provide a secure, scalable, and highly available environment for running containerized applications. 
It provides a range of features, such as automated cluster scaling, automated patching, and integration with other AWS services. 
EKS also provides a range of security features, such as encryption at rest, IAM authentication, and network isolation.

# eksctl

This is a simple CLI tool for creating cluster on EKS - Amazon's new managed kubernetes service for EC2. It is written and uses CloudFormation.

# Configuring & deploying EKS using eksctl.

      EC2 bootstrapping instance
      awscli version 2
      eksctl installation
      eks user and permissions
      kubectl

Just like KOPS am going to use BootStrapping instance, by launching ubuntu EC2 instance.

![eks-bbot](https://user-images.githubusercontent.com/101070055/232343031-114005d2-179b-4c16-b22c-a976bc5d3f17.png)

Install eksctl

      curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
      sudo mv /tmp/eksctl /usr/local/bin
      eksctl version

Install awscli

      curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip awscliv2.zip
      sudo ./aws/install
     
Create eks user and permissios, to keep it simple i use below permissions

      AmazonEC2FullAccess
      IAMFullAccess
      CloudFormationFullAccess
      Systems Manager (Limited: Read & List)
      EKSFullAccess  {create a policy and enable all Access}

Create eks user access key and as well use aws configure to authencticate to configure it

Install kubectl command

        curl -LO https://dl.k8s.io/release/v1.27.0/bin/linux/amd64/kubectl
        sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
        kubectl version --output=yaml
        
![kubctl-v](https://user-images.githubusercontent.com/101070055/232347209-d2ac6177-99fd-40e3-adac-5d57809d18c5.png)
        
I will be deploying the cluster on aws region us-east-2 

Create Cluster

        eksctl create cluster --name eks-cluster --nodes-min=3
        
Create Cluster Specifying node types

            eksctl create cluster --name eks-cluster --nodes-min=3 --nodes-max=5 --node-type=t3.medium

![eks-cluster-up](https://user-images.githubusercontent.com/101070055/232351388-9ffc0372-587a-4d9a-af9d-fc9e641a5fba.png)

![eks-node](https://user-images.githubusercontent.com/101070055/232351218-b1d6aee4-f317-4e53-b895-360c8340d2da.png)

## Ebs-csi-driver

To use EBS Volume as persistent volume in EKS, i need to install EBS-CSI-Driver with required permissions as addons inside the cluster.    

Command to deploy ebs-csi driver

- eksctl Command line: This will use cloud formation stack

            eksctl create iamserviceaccount \
              --name ebs-csi-controller-sa \
              --namespace kube-system \
              --cluster <cluster name> \
              --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
              --approve \
              --role-only \
              --role-name <enter-preffered role name>

- To add the Amazon EBS CSI add-on using eksctl

            eksctl create addon --name aws-ebs-csi-driver --cluster <cluster-name> --service-account-role-arn arn:aws:iam::<account-id>:role/<role-name> --force
            
Updating the Amazon EBS CSI driver as an Amazon EKS add-on

- Check the current version of my Amazon EBS CSI add-on using eksctl

            eksctl get addon --name aws-ebs-csi-driver --cluster <cluster-name>

- Update the add-on to the version returned under UPDATE AVAILABLE in the output 

          eksctl update addon --name aws-ebs-csi-driver --version <ebs-addon-version e.g v1.17.0-eksbuild.1> --cluster <cluster-name> --force
      
- Removing the Amazon EBS CSI add-on "eksctl command'

            eksctl delete addon --cluster <cluster-name> --name aws-ebs-csi-driver --preserve
      
# Ebs-csi addon using aws cli

- View cluster's OIDC provider URL

            aws eks describe-cluster \
              --name <cluster-name> \
              --query "cluster.identity.oidc.issuer" \
              --output text

- Sample output
      
      https://oidc.eks.region-code.amazonaws.com/id/EXAMPLED539D4633E53DE1B71EXAMPLE
      OIDC provider URL ID is "EXAMPLED539D4633E53DE1B71EXAMPLE"

- Create a .json file and edit it with text editor example " touch ebs-csi.json"
      
      touch ebs-csi.json
      vim ebs-csi.json
      
- Paste this code and edit it,
      
                  {
              "Version": "2012-10-17",
              "Statement": [
                {
                  "Effect": "Allow",
                  "Principal": {
                    "Federated": "arn:aws:iam::<account-id>:oidc-provider/<oidc.eks.region-code.amazonaws.com/id/EXAMPLED539D4633E53DE1B71EXAMPLE>"
                  },
                  "Action": "sts:AssumeRoleWithWebIdentity",
                  "Condition": {
                    "StringEquals": {
                      "oidc.eks.<aws-region>.amazonaws.com/id/<OIDC provider URL ID>:aud": "sts.amazonaws.com",
                      "oidc.eks.<aws-region>.amazonaws.com/id/<OIDC provider URL ID>:sub": "system:serviceaccount:kube-system:ebs-csi-controller-sa"
                    }
                  }
                }
              ]
            }
   
save and exit
      
- Create the role .

                  aws iam create-role \
                    --role-name <role-name> \
                    --assume-role-policy-document file://"<json-file-name e.g ebs-csi.json>"

- Attach the required AWS managed policy to the role with the following command   
      
                        aws iam attach-role-policy \
                          --policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
                          --role-name <created-role-name>
 
- Annotate the ebs-csi-controller-sa Kubernetes service account with the ARN of the IAM role

                  kubectl annotate serviceaccount ebs-csi-controller-sa \
                      -n kube-system \
                      eks.amazonaws.com/role-arn=arn:aws:iam::<account-id>:role/<role-name>
      
- Restart the ebs-csi-controller deployment for the annotation to take effect.
      
            kubectl rollout restart deployment ebs-csi-controller -n kube-system
      
- To add the Amazon EBS CSI add-on using the AWS CLI   
      
                        aws eks create-addon --cluster-name <cluster-name> --addon-name aws-ebs-csi-driver \
                    --service-account-role-arn arn:aws:iam::<account-id>:role/<role-name>

- To remove the Amazon EBS CSI add-on using the AWS CLI      

            aws eks delete-addon --cluster-name my-cluster --addon-name aws-ebs-csi-driver --preserve
