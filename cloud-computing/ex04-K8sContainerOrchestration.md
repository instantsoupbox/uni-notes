# Exercise 04 - Kubernetes: Container Orchestration

## Components
* etcd (etc daemon): K8s'backing store, storage of cluster data.
* Master
    * Kube-apiserver: Exposes the K8s API (front-end for the K8s control plane.)
    * Kube-scheduler: Watches newly created pods with no node assigned to them and assigns them a node.
    * Kube-controller-manager: Runs controllers (background threads that handle routine tasks in the cluster)
        * Node controller
        * Replication controller
        * Endpoints controller
* Node / Minion
    * Pod: atomic unit on k8s platform, collection of containers deployed on the same minion.
    * kube-proxy: maintains network rules on the host, performs connection forwarding
    * kubelet: node-level manager running on a minion