## Current setup

Normal deployments for MySQL, but we can use `stateful`, but still for achieving HA, alot of manual work needs to be done

## Drawbacks of MySQL in K8s

* Complex to achieve HA 
	* In k8s docs, we have 1 MySQL master, and other re-duplication replicas
* Complex multi-AZ support for EBS
	* EBS is zone related service, so in multi-AZ we need to find out a way to replicate the data in one AZ to all AZ, in order to access the data in a FL manner.
* Complex Master-Master MySQL setup
* Complex Master-Slave MySQL setup
* No automatic backup & recovery
* No auto-upgrade for MySQL


Using RDS

* High availability 
* Backup & recovery
* Read replicas
* Metrics & monitoring
* Automatic upgrades
* Multi-AZ support

For connecting to RDS : `external name service`
