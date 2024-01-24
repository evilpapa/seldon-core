# Seldon 服务编排器

## 功能性

 * REST and gRPC for Seldon and Tensorflow protocols. Easily extendable to other protocols.
 * Logging of request and or response payloads to arbitrary URLs with CloudEvents
 * Tracing for REST and gRPC
 * Prometheus metrics for REST and gRPC
 * All components must be REST or gRPC in agraph. No mixing.
 * Not meta data additions to payloads are carried out by the executor.


## 测试

一个示例：

```JSON
apiVersion: machinelearning.seldon.io/v1alpha2
kind: SeldonDeployment
metadata:
  labels:
    app: seldon
  name: seldon-model
spec:
  name: test-deployment
  predictors:
  - componentSpecs:
    - spec:
        containers:
        - image: seldonio/mock_classifier_rest:1.3
          name: classifier
    graph:
      children: []
      endpoint:
        type: REST
      name: classifier
      type: MODEL
    labels:
      version: v1
    name: example
    replicas: 1

```
 
