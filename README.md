# aerospike-kube

This project contains the init container used in Kubernetes (k8s) and the aerospike StatefulSet definition

## Usage:

Configure `image/aerospike.conf` for your aerospike cluster.

Build and push the aerospike-install container to the Docker registry of your choice (See image/README.md)

Use the aerospike-install image as the init-container.

**Kubernetes 1.8+ (StatefulSet)**

Deploy your StatefulSet: `kubectl create -f aerospike-statefulset.yaml`

**Kubernetes 1.5 (StatefulSet old format)**

Deploy your StatefulSet: `kubectl create -f aerospike-statefulset.yaml`

*note* The last commit of the old template format is `cff91c3`.  

*note* PetSet, a k8s 1.3 alpha feature, is now StatefulSet, a 1.5 beta feature.

**Kubernetes 1.3 (PetSet)**

Deploy your PetSet: `kubectl create -f aerospike-petset.yaml`



## Requirements

* Kubernetes 1.3+ with alpha features (PetSet, init containers)   
or  
* Kubernetes 1.5+ with beta features (StatefulSet)  
or
* Kubernetes 1.8+
* Kubernetes DNS add-in


## Example

In your yaml:

```
...
spec:
  serviceName: "aerospike"
  replicas: 3
  template:
    metadata:
      labels:
        app: aerospike

      initContainers:
      -  name: aerospike-install
         image: <YOUR_PRIVATE_REPO>/aerospike-install
         volumeMounts:
         - name: confdir
           mountPath: /etc/aerospike
         env:
         - name: POD_NAMESPACE
           valueFrom:
             fieldRef:
               fieldPath: metadata.namespace
      ...
```
