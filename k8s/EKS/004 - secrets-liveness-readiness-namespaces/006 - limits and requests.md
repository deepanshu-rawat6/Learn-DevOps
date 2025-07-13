With the limits and requests ---> kube-scheduler ---> decides on which nodes should the pod be placed

Limits : Resource limit for our container --> kubelet enforces these limits so that the running container do no consume more resources then the limit set

Requests: Reservation of resources ---> kubelet reserves atleast the request amount of that system resources for that container use.