# INGRESS CONTROLLER 

![Ingress-1](https://user-images.githubusercontent.com/101070055/236702320-57df7257-71bd-464c-b6dc-b0744b45d8ac.png)                        ![Ingress-controller](https://user-images.githubusercontent.com/101070055/236702374-906021fd-ad8a-4840-99dc-2c39097a2f9d.png)
   
### WHAT IS INGRESS CONTROLLER ?

An ingress controller acts as a reverse proxy and load balancer. 
It implements a Kubernetes Ingress. The ingress controller adds a layer of abstraction to traffic routing, 
accepting traffic from outside the Kubernetes platform and load balancing it to Pods running inside the platform.

#### Uses

It is the entry point for all incoming traffic to the cluster. 
It is responsible for routing external requests to the correct service and port within the cluster.
With ingress controller kuberentes administrators can implement host based, path based routing inside the cluster.
##### path: /apple       >>> This is example of path based routing
##### host: book.jmcglobal.com    >>> Example of host based routing

### SETTING UP INGRESS CONTROLLER ON MINIKUBE

- Enable ingress addons inside the cluster with 

         minikube addons enable ingress
         
- On ubuntu local machine set up local domain-name
         sudo vim /etc/hosts
- Then paste the minikube ip, and enter any unregistered local domain-name which will be used as host on kubernetes ingress rule

         192.168.10.1    jmcglobal-tech.net

- Apply configure and apply ingress rule 

         kubectl apply -f minikube-ingress-rule.yaml
         
- Depending on the number of pods service running inside the cluster you wish to access from the internet, you can configure multiple rules or path based routing to access those pods.

## AWS INGRESS CONTROLLER

There is no much difference implementing ingress controller while running KOPS or EKS with AWS, all you need to do is to download and apply ingress controller inside the cluster. It will provision loadbalancer along with ingress 

- Download Ingress Controller

         wget https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.6.4/deploy/static/provider/aws/deploy.yaml

- Use kubectl command to apply it inside the cluster

kubectl apply -f deploy.yaml

- Then apply your ingress rule

         kubectl apply -f aws-ingress-rule.yaml
         
### To see nginx config overview

         kubectl exec -it -n ingress-nginx deploy/ingress-nginx-controller cat /etc/nginx/nginx.conf
       
