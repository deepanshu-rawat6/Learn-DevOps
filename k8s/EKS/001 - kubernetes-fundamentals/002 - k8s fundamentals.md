# K8s Fundamentals

## Pod

* Smallest unit in k8s ecosystem
* Single instance of an application

## ReplicaSet

* ReplicaSet maintains a stable set of replica Pods running at any given time.

## Deployments

* Deployments runs multiple replicas of the application and automatically replaces any instances that fail or becomes unresponsive.
* Rollouts & Rollbacks changes to applications
* Suite for `stateless`  applications.

## Service

* Abstraction for pods, providing a stable -> virtual IP(VIP) address.
* kind of load balancer for Pods