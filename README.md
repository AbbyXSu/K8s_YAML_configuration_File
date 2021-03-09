# K8s_YAML_Configuration_File
- K8s configuration file has 3 parts
- the configuration file of service and deployments are written seperately 

## 3 parts of the config file 
- Metadata ----name or/and lable of the component

- Specification
    ----spec are the specific to the kind of the component, it can be replicas, selector and template that nesting the spec/blueprint of the pod

- Status 
    ----automatically generate and added by K8s, when comparing the desired staus defined by the file and actual status of the component (from etcd) to achieve self-healing and update

Yaml file should live along with the codes. This is part of IaC interpretation. 

## Connecting Deployment to Pods

In the metadata, the labels give components like deployment or pod a key-value pair(eg. app:nginx)

In the Spec, pods get the label through the template blueprint under the lables of the key-value pair.

The label is matched by the selector under 'matchLabels'

## Ports in service and pod
Target port of the service should match with the container port of the deployment, port of the service is where the service is listening to  

command use in apply the confg file 
```
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
```
use following command to find out the endpoints of the service (the port that requests are forwarded to) and if the right port has been assigned 
```
kubectl describe service 
kubectl get pod -o wide
```
Below command can generate a yaml config file of the component including these that K8s automatically generated and the status of the component(deployment/service)
```
kubectl get deployment ngnix-deployment -o yaml >nginx-deployment-result.yaml
```
# Namespace
purpose: 
- organise resources in namespaces
- virtual cluster in a K8s cluster

command below give a avaliable namespace that K8s offer
```
kubectl get namespace
```
- kubernetes-dashboad only apply to minikube, will not appear in standard cluster
- kube-system contains systerm process, itself can not be create or modify
- kub-pulbic contain a configMap with cluster information that are accessiable publicely  without validation 
- kube-node-lease determines the availability of a node , each node has associated lease object in namespace 
- default - Namespace that created by K8s by default

## Create and delete Namespace 
```
kubectl create namespace [namespace_name]
kubectl delete ns [namespace_name]
kubectl apply -f mysql-configmap.yaml  --namespace= [namespace_name]
```
or define namespace: [namespace_name] in the configMap file under metadata and run 
```
kubectl apply -f mysql-configmap.yaml
```


Namespace is suitable for large scale application that are more than 10 users. it helps to group resource in logical order under Namespaces for complex application, different deployment enviorments and complicated team structure; providing shared resources for one and the other enviorments or team without setting up a seperate cluster and repeating procedures. It also helps to manage the resource that namespace used.

Each NS must has its own config and secret file even it refer to the same resource it used.
However, persisting volume and node live globally in a cluster and cannot be isolated, NS cannot create Volume /node component.

```
kubectl api-resources --namespaced=flase
```
this command shows the resouces that are not binded by namespaces 
```
kubectl api-resources --namespaced=true
```
this command shows the resouces that are binded by namespaces 

# Ingress
Ingress handles requests from the client and forward /redirect to the internal service and pods

Host requirement:
- host : xxx.com  hostname has to be a valid domain address
- map domain name to Node's IP address, which is the entrypoint

## Implementation for Ingress

Ingress Controller Pad needing to be installed to execute and control the ingress, evaluates all the rules, manages redirections and define entrypoint to cluster

Cloud based application might has Cloud Load Balancer that can process the requests before redirecting them to the K8s, which provided an out of the box ingress solution. Benefits is that it is easier to set up with little efforts. 

Command use to start K8s Nginx implementation of Ingress Controller (minikube only)
```
minikube addons enable ingress
```
```
get ingress -n [namespaces-name]
get ingress -n [namespaces-name] --watch
```
use command above to see the address assigned to the Hostname within the namespace of the ingress
```
sudo vim /etc/hosts 
>>> define mapping by addisng IP address and hostname in the file and save 
```
This command map the the IP address to the hostname locally , making it accessible on the local browser via defined hostname 
```
kubectl describe ingress [ingress_name] -n [namespace_name]
```
This command shows the status of the ingress, showing the default backend that handle the requests if the requests are not assigned to the right internal services , interanl service can be create for this dafault backend for showing meangful error messages 

Mulitple paths can be defined under the paths section for the same host.

Alternatively, you can have multiple hosts with multiole sub-domains or domains to redirect requests to different domains and its services.