## Kubernetes config for,
- Kubernetes is an open source container-orchestration system for automating deployment, scaling, and management of containerized applications. It groups containers that make up an application into logical units for easy management and discovery. Kubernetes is designed to work with a range of container tools, including Docker, rkt, and CRI-O. It can be deployed on-premises, in the cloud, or even across multiple clouds. Kubernetes is used to manage containerized applications in a clustered environment, providing basic mechanisms for deployment, maintenance, and scaling of applications. Kubernetes also provides a platform for deploying and managing applications in containers, allowing developers to focus on writing code without worrying about underlying infrastructure. Kubernetes is designed to be highly extensible and provides support for a wide variety of containerized workloads.

## Deployment yaml file
- Kubernetes Deployment is an object that describes how an application should be deployed on a Kubernetes cluster. It describes how many replicas of an application should be running, which resources should be allocated, and where the application should be deployed. Deployment objects are used to declaratively manage the desired state of an application, rolling out updates and providing high availability for applications running on Kubernetes.

## Service Yaml file
- Kubernetes Service files are configuration files that describe how a Kubernetes service should be defined. A Service file defines the type of service, the ports that should be exposed, the selector used to determine which pods should be part of the service, and other information. A Service file is used by the kube-controller-manager to create and manage the service.

## Storage Yaml file:
- Storage class
- Persistent Volume
- Persistent Volume claim
 
## local testing with minikube.

Prerequisite

        Docker engine 
        kubectl
        minikube 

Host machine prerequisite

        minimum of 2cpu
        minimum of 4gb memory
        Update your linux terminal package managers

Install docker.

        sudo apt install docker.io

install minikube 

        curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
        sudo install minikube-linux-amd64 /usr/local/bin/minikube

Install Kubectl >>> follow the link below

        https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

To startup the minikube 

        minikube start --memory 4096 --driver=docker
        
## Referance command to run deeploy and troubleshoot in kubernetes 

       minikube status  >> status of minikube
       minikube stop  >> stop minikube service
       minikube ip >> display minikube IP
       minikube dashboard >> display minikube GUI interface

Basic Kubectl Commands to diagonise and troubleshhot 

      kubectl get pod  >> display pods on default namespace
      kubectl get deployment >> display deployment default namespace
      kubectl get service  >> Display service on default namespace
      kubectl get sc   >> display storage class on kubernetes  (storageClass)
      kubectl get pv  >> display persistent volume     (persistentVolume)
      kubectl get pvc   >> display persistent volume claim  (persistentVolumeClaim)
      kubetl get rs >> display replicaset   (replicaSet)
      kubectl describe <pod/service/deployment/rs/pv/pvc/sc> <name>  >> To view detailed information about the objects
      kubectl logs <pod name> >> to view detailed logs of the pod
      kubectl get all  >> get all objects on default namespace
      
Deployment Basic kubectl commands

   kubectl apply -f .   >> apply every yaml file on the current directory to kubernetes
   kubectl apply -f <yaml file>  
   kubectl delete -f . >> delete from kubernetes every configuration on yaml file
   kubectl delete -f <yaml file> apply per yaml file

`

