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

