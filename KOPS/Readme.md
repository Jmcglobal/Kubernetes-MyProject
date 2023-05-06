## KOPS - Kubernetes Operations

Kops (Kubernetes Operations) is an open source command-line tool for managing production-grade Kubernetes clusters on Amazon Web Services (AWS). It is designed to simplify the process of creating, updating, and maintaining Kubernetes clusters in the cloud. Kops provides a number of features to help you manage your Kubernetes clusters, including:

• Automated cluster creation and management: Kops automates the process of creating, updating, and maintaining Kubernetes clusters on AWS.

• High availability: Kops supports high availability clusters, allowing you to run multiple replicas of your cluster across multiple availability zones for maximum uptime.

• Security and compliance: Kops supports secure, compliant Kubernetes clusters with built-in support for encryption, authentication, authorization, and auditing.

• Scalability: Kops supports both horizontal and vertical scaling of your cluster, allowing you to quickly and easily scale up or down as needed.

• Cost optimization: Kops helps you optimize your costs by allowing you to create and manage clusters with the right resources for your application.

## Getting started  ..

I wont do this configuration on my local terminal, the best way i have always do the configuration is launch an EC2 instance as bootstrap server.

Therefore i have used an EC2 instance as bootstrap server.

![Kops-instance](https://user-images.githubusercontent.com/101070055/232312851-fb308db0-7136-446d-9ef5-61fc47c9bcab.png)

## prerequisite for using ubuntu instance

        update and upgrade instace 
        install awscli and unzip
        install kops
        install kubectl
        Create a group for kops
        Create a dedicated kops user and add user to the group
        Configure AccessKey for user
        Configure User credentials on the KOPS BootStrap Instance.
        create S3 bucket

## Installing kops on ubuntu

            curl -Lo kops https://github.com/kubernetes/kops/releases/download/$(curl  -s  https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
            chmod +x kops
            sudo mv kops /usr/local/bin/kops

             Forother machine /windows/guideline follow this link https://kops.sigs.k8s.io/getting_started/install/
             
Install kubectl

        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
        sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

Check kubectl version

      kubectl version --output=yaml

Guideline link for other installation on kubectl

      https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management
             
Dedicated kops user permissions

      AmazonEC2FullAccess
      AmazonRoute53FullAccess
      Amazons3FullAccess
      AmazonVPCFullAccess
      IAMFullAccess
      AmazonSQSFullAccess
      AmazonEventBridgeFullAccess

![kops-access](https://user-images.githubusercontent.com/101070055/232316021-2fc2e18b-d922-4878-a550-2e75aeeff226.png)

Configure user Access-key and configure it on bootstrap server.

Create S3 bucket

        my bucket-name for this project "kops-bucket-jmc"
        
## Set up the cluster

Create Evironment varibale

      export NAME=kopscluster.k8s.local
      export KOPS_STATE_STORE=s3://kops-bucket-jmc

Create cluster configuration

- I will have to know the number of availability zones available on the region am deploying the cluster, here i am using us-east-2 region, therefore i will run below command to view number of availability zones.

        aws ec2 describe-availability-zones --region us-east-2
        
        availability zones on this region [" us-east-2a","us-east-2b","us-east-2c","us-east-2d"].
        
Kops Command to create cluster/generate cluster configuration file

        kops create cluster --zones us-east-2a,us-east-2b,us-east-2c ${NAME}

To edit cluster configuration details,

list clusters and instance groups created be default

        kops get cluster
        kops get ig --name ${NAME}

![kops-cluster](https://user-images.githubusercontent.com/101070055/232327298-fff2993d-9194-481d-8434-92d4b0c09cc7.png)

Edit cluster with

        kops edit cluster ${NAME}

Edit your node instance group

        kops edit ig --name ${NAME} nodes-us-east-2a

Edit your control-plane instance group

      kops edit ig --name ${NAME} control-plane-us-east-2a

Finally configure your cluster with:

      kops update cluster ${NAME} --yes --admin=87600h

To check if cluster have been deloyed

      kops validate cluster
      
 ![kops-up](https://user-images.githubusercontent.com/101070055/232328660-758a47a6-8f7b-487b-a8be-70876e9c4a3b.png)

As it display above, the cluster is now ready.

![up-kops](https://user-images.githubusercontent.com/101070055/232330402-838be8a9-bd54-4214-add0-4a8aeeb130a9.png)

Be ware those nodes are being protected by Auto scaling group, so that if by any reason one of node crash, auto scaling will rescheduled the instance.

And also, Target group and loadbalancer will be automatically be created for communication between master and nodes .

I can now run kubectl command to deploy, troubleshoot and anallyse microservice.

- To delete KOPS Cluster.

                echo ${NAME}  >> The cluster name evironment variable
                echo ${KOPS_STATE_STORE} >> bucket storage that holds all kops running data and config
                kops delete cluster ${NAME} --yes
                
Whereby the cluster and storage environment variable is lost or not showing when echo command is used, then recreate them again

      export NAME=kopscluster.k8s.local
      export KOPS_STATE_STORE=s3://kops-bucket-jmc
      
With the delete command, it will delete all the cluster and every components it deployed.

- To restart the cluster back.

You must follow the previous step starting from setting the cluster name and storage environment variable.

## CREATE KOPS CLUSTER USING A FEW LINES OF COMMAND

#### Create an S3 Bucket

                aws s3api create-bucket --bucket <enter-bucket-name> --region <Enter-your-chosen-aws-region>
        
#### Create a KOPS Cluster
        
                kops create cluster --name=<enter-cluster-name>.k8s.local --state=s3://<bucket-name> --zones=<availability-zone name> --node-count=1 --node-size=t2.micro --master-size=t2.micro --master-volume-size=8 --node-volume-size=8
        
### Edit Cluster Configuration
        
                kops edit cluster <cluster name>
               
### Build the cluster
        
                kops update cluster <cluster name> --yes
                
### Set Environment variable , cluster name and bucket name

                export KOPS_STATE_STORE=s3://<bucket-name>
                export NAME=<cluster-name>.k8s.local
        
### Experiment with few commands

kops get cluster
kops get ig --name $NAME
kops edit cluster $NAME


        
The Kops Cluster will be created, and you can see the whole configuration on aws environment, Autoscaling, targets grropu, loadbalancer, and EC2 instances for nodes and master. the configurtaion state files is stored on s3 bucket.
        
### To deploy microservices, ensure kubectl is installed as well

Am available on via Email for any enquiry ..

Mmadubugwuchibuife@gmail.com
                
