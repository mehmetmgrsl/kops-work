## Why kOps (Kubernetes Operations)?

- To automate the management of Kubernetes clusters. For example, kOps can create, apply, and update cluster configurations.(3*)

## Install kops

```
curl -Lo kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64

chmod +x kops

sudo mv kops /usr/local/bin/kops
```

## Add the permissions below to your IAM user

- AmazonVPCFullAccess
- AmazonEC2FullAccess
- AmazonS3FullAccess
- IAMFullAccess
- AmazonRoute53FullAccess
- AmazonSQSFullAccess
- AmazonEventBridgeFullAccess

## Create a S3 bucket for storing kOps objects

```aws s3api create-bucket --bucket kops-mgrsl-storage --region eu-north-1 --create-bucket-configuration LocationConstraint=eu-north-1```

## Creating your first cluster

### Prepare local environment¶

- export NAME=mgrsl-cluster.k8s.local
- export KOPS_STATE_STORE=s3://kops-mgrsl-storage

### Create cluster configuration

```
kops create cluster \
    --name=${NAME} \
    --cloud=aws \
    --zones=eu-north-1a \
    --discovery-store=${KOPS_STATE_STORE}/${NAME}/discovery \
    --node-count=1 \
    --node-size=t3.micro \
    --master-size=t3.micro \
    --master-volume-size=8 \
    --node-volume-size=8
```

### Build the Cluster

```kops update cluster --name ${NAME} --yes --admin```

### Verify the installation

```kops validate cluster ${NAME}```

### Use the Cluster

```kubectl get nodes```

### Delete the cluster

```kops delete cluster --name ${NAME} --yes```


### Resources
1. [kOps - Kubernetes Operations](https://kops.sigs.k8s.io/)
2. [How to Manage Hundreds of Kubernetes clusters | KOPS  - Abhishek Veeramalla](https://www.youtube.com/watch?v=44Qk55E6CAA)
3. [Kubernetes kOps: Step-By-Step Example & Alternatives](https://blog.kubecost.com/blog/kubernetes-kops/)