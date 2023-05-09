# Prometheus - Grafana

![prom](https://github.com/Jmcglobal/Kubernetes-MyProject/assets/101070055/d2ab5c59-4587-41f3-8623-880b21c9a983)

### Why Prometheus and Grafana ?
##### Prometheus is a time-series database and monitoring system. It collects metrics through Kubernetes cluster API server at given intervals, evaluates rule expressions, displays the results, and can trigger alerts if some condition is observed to be true. Prometheus is written in Go and uses a highly dimensional data model. It is designed to be horizontally scalable, reliable, and easy to manage.

#### Grafana is an open-source visualization and analytics platform. It allows users to create, explore, and share dashboards to query, visualize, and alert on data. It supports a wide range of data sources, including Prometheus, and can be used to create complex monitoring dashboards. Grafana also provides alerting, so users can be notified when certain conditions are met.

Putting them together, Prometheus and Grafana provide a powerful monitoring solution for the Kubernetes cluster. Prometheus collects metrics from configured targets, stores them in its time-series database, and evaluates rule expressions. Grafana then queries the Prometheus database and visualizes the results in a dashboard. Alerts can also be configured in Grafana, so users can be notified when certain conditions are met. This makes it easy to monitor the performance of IT infrastructure and take action if necessary.

### Prometheus Installation (Helm)
Ensure kubernetes cluster is up and running and as well install heml command (helm is a package manager for kubernetes)

- Install helm command

        wget https://get.helm.sh/helm-v3.8.2-linux-amd64.tar.gz
        tar xvf helm-*-linux-amd64.tar.gz
        sudo mv linux-amd64/helm /usr/local/bin

- Confirm Helm Installation

        helm version

- Get Prometheus with helm into helm repo

        helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

- Update helm repo

        helm repo update

- View helm repo

        helm repo ls
        
- Install Prometheus inside cluster

        helm install prometheus prrces ometheus-community/prometheus

![helm](https://github.com/Jmcglobal/Kubernetes-MyProject/assets/101070055/944cd1c6-c518-4630-bf45-929c71661507)

- Confirm if Prometheus is running on the cluster

        kubectl get all

- This command will display entire resources on default namespace, including service and prometheus is deployed on default namespace.

![get-all](https://github.com/Jmcglobal/Kubernetes-MyProject/assets/101070055/1c755221-d606-4b5c-a7ee-28b2d144e95c)


