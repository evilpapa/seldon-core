# seldon-single-model

![Version: 0.2.0](https://img.shields.io/static/v1?label=Version&message=0.2.0&color=informational&style=flat-square)

Chart to deploy a machine learning model in Seldon Core.

## Usage

使用本chart，首先要添加 `seldonio` Helm 仓库：

```bash
helm repo add seldonio https://storage.googleapis.com/seldon-charts
helm repo update
```

一旦完成，你可以按照如下使用预估图模板：

```bash
helm template $MY_MODEL_NAME seldonio/seldon-single-model --namespace $MODELS_NAMESPACE
```

注意你也可以直接部署预估图到集群中：
使用：

```bash
helm install $MY_MODEL_NAME seldonio/seldon-single-model --namespace $MODELS_NAMESPACE
```

**Homepage:** <https://github.com/SeldonIO/seldon-core>

## 源码

* <https://github.com/SeldonIO/seldon-core>
* <https://github.com/SeldonIO/seldon-core/tree/master/helm-charts/seldon-single-model>

## 设置值

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| annotations | object | `{}` | Annotations applied to the deployment |
| apiVersion | string | `"machinelearning.seldon.io/v1"` | Version of the SeldonDeployment CRD |
| hpa.enabled | bool | `false` | Whether to add an HPA spec to the deployment |
| hpa.maxReplicas | int | `5` | Maximum number of replicas for HPA |
| hpa.metrics | list | `[{"resource":{"name":"cpu","targetAverageUtilization":10},"type":"Resource"}]` | Metrics that autoscaler should check |
| hpa.minReplicas | int | `1` | Minimum number of replicas for HPA |
| labels | object | `{}` | Labels applied to the deployment |
| model.env | object | `{"LOG_LEVEL":"INFO"}` | Environment variables injected into the model's container |
| model.envSecretRefName | string | `""` | The model secret name for enviroment variables |
| model.image | string | `""` | Docker image used by the model |
| model.implementation | string | `""` | Implementation of Prepackaged Model Server |
| model.logger.enabled | bool | `false` |  |
| model.logger.url | string | `""` |  |
| model.mlflow.xtype | string | `""` |  |
| model.resources | object | `{"requests":{"memory":"1Mi"}}` | Resource requests and limits for the model's container |
| model.uri | string | `""` | Model's URI for prepackaged model server |
| protocol | string | `"seldon"` |  |
| replicas | int | `1` | Number of replicas for the predictor |
