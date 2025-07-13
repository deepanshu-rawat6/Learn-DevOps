
Creating a policy first -> for Amazon_EBS_CSI_Driver

```json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:AttachVolume",
        "ec2:CreateSnapshot",
        "ec2:CreateTags",
        "ec2:CreateVolume",
        "ec2:DeleteSnapshot",
        "ec2:DeleteTags",
        "ec2:DeleteVolume",
        "ec2:DescribeInstances",
        "ec2:DescribeSnapshots",
        "ec2:DescribeTags",
        "ec2:DescribeVolumes",
        "ec2:DetachVolume"
      ],
      "Resource": "*"
    }
  ]
}
```

Fetch the IAM role arn, associated with our worker nodes:

```bash
kubectl -n kube-system describe configmap aws-auth
```

Attaching the created policy to the IAM role

Now, apply the EBS CSI driver manifest file:

Using kustomize:

```shell
kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.45"
```

Using helm:

```shell
helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver
helm repo update
```

Installing driver:

```shell
helm upgrade --install aws-ebs-csi-driver \
    --namespace kube-system \
    aws-ebs-csi-driver/aws-ebs-csi-driver
```


Verify the installation:

```shell
kubectl get pods -n kube-system -l app.kubernetes.io/name=aws-ebs-csi-driver
```

volumeBindingMode: WaitForConsumer (wait or delay the volume binding and provisioning of a PersistentVolume until a Pod using the PVC is created)

Otherwise, maybe a Pod is created in different AZ and the vol already present in a different AZ (inter-AZ cost)

Examples for EBS CSI driver: https://github.com/kubernetes-sigs/aws-ebs-csi-driver/tree/master/examples/kubernetes

For a NodePort svc:

Get the  nodes, external IP:

```shell
kubectl get nodes -o wide
```

```shell
kubectl get svc,pv,pvc
```