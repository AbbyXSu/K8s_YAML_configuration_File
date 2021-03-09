# K8s_YAML_Configuration_File
- K8s configuration file has 3 parts
- the configuration file of service and deployments are written seperately 

## 3 parts of the config file 
- Metadata ----name or/and lable of the component

- Specification
    ----spec are the specific to the kind of the component, it can be replicas, selector and template that nesting the spec of pod

- Status 
    ----automatically generate and added by K8s, when comparing the desired staus defined by the file and actual status of the component (from etcd) to achieve self-healing and update

Yaml file should live along with the codes. This is part of IaC interpretation. 

## Connecting Deployment to Pods
