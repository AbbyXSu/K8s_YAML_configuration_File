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

