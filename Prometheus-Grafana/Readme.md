# Prometheus - Grafana

![prom](https://github.com/Jmcglobal/Kubernetes-MyProject/assets/101070055/d2ab5c59-4587-41f3-8623-880b21c9a983)

### Why Prometheus and Grafana ?
##### Prometheus is a time-series database and monitoring system. It collects metrics through Kubernetes cluster API server at given intervals, evaluates rule expressions, displays the results, and can trigger alerts if some condition is observed to be true. Prometheus is written in Go and uses a highly dimensional data model. It is designed to be horizontally scalable, reliable, and easy to manage.

#### Grafana is an open-source visualization and analytics platform. It allows users to create, explore, and share dashboards to query, visualize, and alert on data. It supports a wide range of data sources, including Prometheus, and can be used to create complex monitoring dashboards. Grafana also provides alerting, so users can be notified when certain conditions are met.

##### Putting them together, Prometheus and Grafana provide a powerful monitoring solution for the Kubernetes cluster. Prometheus collects metrics from configured targets, stores them in its time-series database, and evaluates rule expressions. Grafana then queries the Prometheus database and visualizes the results in a dashboard. Alerts can also be configured in Grafana, so users can be notified when certain conditions are met. This makes it easy to monitor the performance of IT infrastructure and take action if necessary.
