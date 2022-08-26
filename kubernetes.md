# Kubernetes

## Useful commands

### See what ports are in use by which pod
`k3s kubectl get services -A`
![Kubernetes ports](./images/ark_k3s_ports.png "Kubernetes ports in use")
_Make sure the same port number is not used by multiple pods!_

### See what pods are deployed
`k3s kubectl get pods -A`
![Kubernetes pods](./images/ark_k3s_pods.png "Kubernetes deployed pods")
