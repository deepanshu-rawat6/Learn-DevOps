Few pointers:

* Runs before the App containers
* Can container utils or setup scripts which might not be present in the app image
* Can run multiple init containers before app containers
* Init containers == regular containers, except:
	* Run to completion
	* Each must completely run successfully before the next starts.
* If init containers == failed, k8s repeat till the init containers.status == success
	* However, if the pod has the restartPolicy == never, k8s does not restart the pod