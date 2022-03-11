# Kubernetes

## Kubectl

### Get
```yml
# list resources
kubectl get 
# for more information
kubectl get <option> -o wide
# list resources per namespace
kubectl get <option> --all-namespaces

```
### Describe
```yml
# view details
kubectl describe 
```
### Create
```yml
# create a namespace
kubectl create ns
# create a resource from JSON or YAML file:
kubectl create -f <filename>
# blueprint for creating pods
kubectl create deployment <name> --image=<image_name>
```
### Apply
```yml
# applying and updating a resource
kubectl apply -f <service_name>
```
### Delete
```yml
# to remove a resource
kubectl delete <something>
```
### Exec
```yml
# start a shell session
kubectl <pod_name> -it -- <command>
```
### Logs
```yml
# print logs
kubectl logs <pod_name>
# stream logs
kubectl logs -f <pod_name>
```
### Miscelleanous
```yml
# display addresses of the control plane and services
kubectl cluster-info
# display merged kubeconfig settings or a specified kubeconfig file.
kubectl config view
# specify a namespace
kubectl -n <namespace_name> <flags> <options>
# auto generated configuration files with default configs for already deployed apps
kubectl edit deployment <name>
# encrypt environment variable
echo -n "<unencrypt_name>" | base64
```
## Shortcut
|Short Name|Long Name|
|:----|:----|
|csr|certificatesigningrequests|
|cs|componentstatuses|
|cm|configmaps|
|ds|daemonsets|
|deploy|deployments|
|ep|endpoints|
|ev|events|
|hpa|horizontalpodautoscalers|
|ing|ingresses|
|limits|limitranges|
|ns|namespaces|
|no|nodes|
|pvc|persistentvolumeclaims|
|pv|persistentvolumes|
|po|pods|
|pdb|poddisruptionbudgets|
|psp|podsecuritypolicies|
|rs|replicasets|
|rc|replicationcontrollers|
|quota|resourcequotas|
|sa|serviceaccounts|
|svc|services|




