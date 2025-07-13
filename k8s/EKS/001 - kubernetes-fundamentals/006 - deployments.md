Deployments types:

*  Create a Deployment to rollout a ReplicaSet
* Updating the deployment
* Rollback to deployment
* Scaling a deployment
* Pausing and resuming a deployment
* Deployment status
* Clean up policy (updation of deployment, so it maintain about 10 versions of the deployments or add additional versions by adding additional tags in the YAML under the spec of the deployment)
* Canary Deployments (So, if we want to add a new version of our application in the live production traffic and the traffic should distribute between the old and new version of our application)

### Deployment : Intro

```bash
kubectl create deployment <Deployment-Name> --image=<Container-Image>
```

