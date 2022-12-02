# Kubernetes

## Useful commands

### See what ports are in use by which pod
`k3s kubectl get services -A`
![Kubernetes ports](./images/ark_k3s_ports.png "Kubernetes ports in use")
_Make sure the same port number is not used by multiple pods!_

### See what pods are deployed
`k3s kubectl get pods -A`
![Kubernetes pods](./images/ark_k3s_pods.png "Kubernetes deployed pods")

### Lots of details on the pods and containers
`k3s kubectl get pods,svc,daemonsets,deployments,statefulset,sc,pvc,ns,job --all-namespaces -o wide`

### See state of deployment of pods for given namespaces
`k3s kubectl get pods -n ix-ark2 -o wide`
Also useful with `watch` to see changes. 
When all pods are _READY_, the status in the GUI will go to _ACTIVE_ for the app.
![Kubernetes pods status](./images/ark_k3s_pods_status.png "Kubernetes pods status for namespace")

### Check resource limits (e.g. to see what the memory limit is)
`k3s kubectl describe nodes`

### Check ports in use for given namespace or all namespaces

For namespace _ix-ark01-theisland_
`k3s kubectl get services -n ix-ark01-theisland --output=json | jq .items[].spec.ports[]`

For all namespaces
`k3s kubectl get services --all-namespaces --output=json | jq .items[].spec.ports[]`

### List all containerIDs of initContainer of all pods
Helpful when cleaning up stopped containers, while avoiding removal of initContainers.
`k3s kubectl get pods --all-namespaces -o jsonpath='{range .items[*].status.initContainerStatuses[*]}{.containerID}{"\n"}{end}' | cut -d/ -f3`

### List container IDs by namespace and name

`k3s kubectl get pods --all-namespaces --output=json | jq -r '.items | sort_by(.metadata.namespace,.metadata.name)[]|[.metadata.uid, .metadata.creationTimestamp, .metadata.namespace, .metadata.name] | @tsv'`

To find orphaned pods, check pods in _/var/lib/kubelet/pods_

![Kubernetes pods list](./images/ark_k3s_ids.png "Kubernetes pods list for namespace")


`k3s kubectl get services -n ix-ark02-theisland --output=json | jq -r ' ["name", "nodePort", "port", "protocol", "targetPort"],  (.items[] | .spec.ports[] | [.name, .nodePort, .port, .protocol, .targetPort]) | @tsv'`