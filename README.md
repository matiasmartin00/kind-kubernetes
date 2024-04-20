# Testing kind

install 

brew install kind

# Multi node

## Create

kind create cluster --config kind-multi-node-config.yml --kubeconfig $TMPDIR/kind-multi-node-config

## Verify

kubectl get nodes --kubeconfig $TMPDIR/kind-multi-node-config

## Delete

kind get clusters
multi-node-cluster

kind delete cluster --name multi-node-cluster

# Multine node and control-planes

## Create

kind create cluster --config kind-ha-multi-node-config.yml --kubeconfig $TMPDIR/kind-ha-multi-node-config

## Verify

kubectl get nodes --kubeconfig $TMPDIR/ha-kind-multi-node-config

## Echo server

### Creating deployment

kubectl create deployment echo --image=g1g1/echo-server:0.1 --kubeconfig $TMPDIR/kind-ha-multi-node-config

### Exposing deployment

kubectl expose deployment echo --type=NodePort --port=7070 --kubeconfig $TMPDIR/kind-ha-multi-node-config

### Creating proxy

kubectl proxy --kubeconfig $TMPDIR/kind-ha-multi-node-config

### Testing

curl http://localhost:8001/api/v1/namespaces/default/services/echo:7070/proxy/hello-world

### Delete

kind get clusters
ha-multi-node-cluster

kind delete cluster --name ha-multi-node-cluster