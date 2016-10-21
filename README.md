# aerospike-kube

This project contains the init container used in Kubernetes (k8s) and the aerospike petset definition

## Usage:

Configure `image/aerospike.conf` for your aerospike cluster.

Build and push the aerospike-install container to the Docker registry of your choice (See image/README.md)

Use the aerospike-install image as the init-container for the PetSet

## Requirements

* Kubernetes 1.3+ with alpha features (PetSet, init containers)
* Kubernetes DNS add-in


## Example

In your kubernetes PetSet yaml:

```
spec:
  serviceName: "aerospike"
  replicas: 3
  template:
    metadata:
      labels:
        app: aerospike
      annotations:
        pod.alpha.kubernetes.io/initialized: "true"
        pod.alpha.kubernetes.io/init-containers: '[
          {
             "name": "install",
             "image": "<YOUR_DOCKER_REGISTRY>/aerospike-install",
             "env": [
                  {
                      "name": "POD_NAMESPACE",
                      "valueFrom": {
                          "fieldRef": {
                              "apiVersion": "v1",
                              "fieldPath": "metadata.namespace"
                          }
                      }
                   }
                ],
             "volumeMounts": [
               {
                 "name":"confdir",
                 "mountPath": "/etc/aerospike"
               }
             ]
          }
        ]'
...
```


