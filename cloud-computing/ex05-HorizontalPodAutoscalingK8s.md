# Exercise 05 - Horizontal Pod Autoscaling in Kubernetes

## Autoscaling
* The amount of computational resources in a server farm is scaled automatically based on the load on the farm.
* Amount of computational resouces: typically measured in terms of the number of active servers.

### Advantages
* On-premises web server infrastructure:
    * Green computing
    * Replacing unhealthy instances
* Off-site hosting in the cloud:
    * Lower bills
    * Better handling of unexpected traffic spikes.

## Horizontal Pod Autoscaler
* Task: Automatic scaling of the the number of ports in a `{ replica controller, deployment, replica set }` based on observed CPU utilization. 

### Autoscaling Algorithm
* Periodically query of pods with are part of the **scale subresource**, collect their CPU utilization
* Compare arithmetic mean of the pods' CPU utilization with defined target.
* **CPU utilization** defined by recent CPU usage of a pod (average over the last 1 minute) divided by CPU request by pod
* Adjust replicas of the scale.

## Heapster
* Container cluster monitoring, performance analysis for k8s.
* Collection, interpretation of various signals like compute resource usage, lifecycle events etc.

## InfluxDB
* High-availability storage and retrieval of time series data in fields such as `{ operations, monitoring, application metrics, IoT sensor data, real-time analytics }`