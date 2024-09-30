# Portainer on Kubernetes

These deployment manifests will deploy Portainer inside the `portainer` namespace. Portainer uses this namespace to store system information, as such this namespace must not be changed.

## Deploy Portainer inside your cluster and access it via node port

You can use the following commands to deploy Portainer:

```
git clone https://github.com/SumonPaul18/k8s-portainer.git
kubectl apply -f k8s-portainer/portainer-deploy.yaml
kubectl apply -f k8s-portainer/portainer-nfs-pv-pvc.yaml
```
```
kubectl get all -n portainer1
```
Delete NameSpace portainer1
```
kubectl delete ns portainer1
```

This will deploy the Portainer application and create an external load balancer which you'll be able to use to access Portainer on port 9000.

## Deploy Portainer inside your cluster and access it via node port

If you prefer to access Portainer via a specific port on a node of your cluster, use the following commands:

```
curl -LO https://raw.githubusercontent.com/portainer/portainer-k8s/master/portainer-nodeport.yaml
kubectl apply -f portainer-nodeport.yaml
```

This will expose Portainer on the port `30777` inside your cluster (`30776` for Edge tunnel server). You can change these ports inside the manifest if you wish.

## Deploy Portainer using Helm Chart

Run the following commands to install Portainer via `helm`:

```
kubectl create namespace portainer
helm repo add portainer http://portainer.github.io/portainer-k8s
helm upgrade --atomic -i portainer portainer/portainer-beta --version 1.0.0 -n portainer
```

**Note**: this deployment defaults to exposing Portainer over an external load balancer, have a look at the [chart configuration](charts/portainer-beta/README.md) in the `charts/portainer-beta` folder for more information on how to configure the helm deployment.

## Update to a new version of the beta

In order to update to the latest version of the beta, you'll need to delete the `portainer` namespace and redeploy it.

```
kubectl delete namespace/portainer
kubectl apply -f portainer.yaml // or kubectl apply -f portainer-nodeport.yaml based on your initial deployment
```

## Manage a remote Kubernetes cluster

In order to manage a remote Kubernetes cluster, you'll need a Portainer for Kubernetes BETA instance already deployed inside a Kubernetes cluster and connect it to a Portainer agent running inside the remote cluster.

### Deploy the Portainer agent and access it via an external load balancer

If your cloud provider supports external load balancers, you can use the following command to deploy the regular Portainer agent (not Edge):

```
curl -LO https://raw.githubusercontent.com/portainer/portainer-k8s/master/agent/portainer-agent.yaml
kubectl apply -f agent/portainer-agent.yaml
```

This will deploy the Portainer agent and create an external load balancer which you'll be able to use to connect to the agent on port 9001.

# Reference

Portainer 2.0 includes support for Kubernetes, head to https://www.portainer.io/installation/ for more details about how to install it.
