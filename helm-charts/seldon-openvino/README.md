# seldon-openvino

> **:exclamation: This Helm Chart is deprecated!**

![Version: 0.1](https://img.shields.io/static/v1?label=Version&message=0.1&color=informational&style=flat-square)

Proxy integration to deploy models optimized for Intel OpenVINO in Seldon Core

## Usage

使用本chart，首先要添加 `seldonio` Helm 仓库：

```bash
helm repo add seldonio https://storage.googleapis.com/seldon-charts
helm repo update
```

一旦完成，你可以按照如下使用预估图模板：

```bash
helm template $MY_MODEL_NAME seldonio/seldon-openvino --namespace $MODELS_NAMESPACE
```

注意你也可以直接部署预估图到集群中：
使用：

```bash
helm install $MY_MODEL_NAME seldonio/seldon-openvino --namespace $MODELS_NAMESPACE
```

## 源码

* <https://github.com/SeldonIO/seldon-core>
* <https://github.com/SeldonIO/seldon-core/tree/master/helm-charts/seldon-openvino>

## 设置值

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| engine.env.SELDON_LOG_MESSAGES_EXTERNALLY | bool | `false` |  |
| engine.env.SELDON_LOG_REQUESTS | bool | `false` |  |
| engine.env.SELDON_LOG_RESPONSES | bool | `false` |  |
| engine.resources.requests.cpu | string | `"0.1"` |  |
| openvino.image | string | `"intelaipg/openvino-model-server:0.3"` |  |
| openvino.model.env.LOG_LEVEL | string | `"DEBUG"` |  |
| openvino.model.input | string | `"data"` |  |
| openvino.model.name | string | `"squeezenet1.1"` |  |
| openvino.model.output | string | `"prob"` |  |
| openvino.model.path | string | `"/opt/ml/squeezenet"` |  |
| openvino.model.resources | object | `{}` |  |
| openvino.model.src | string | `"gs://seldon-models/openvino/squeezenet"` |  |
| openvino.model_volume | string | `"hostPath"` |  |
| openvino.port | int | `8001` |  |
| predictorLabels.fluentd | string | `"true"` |  |
| predictorLabels.version | string | `"v1"` |  |
| sdepLabels.app | string | `"seldon"` |  |
| tfserving_proxy.image | string | `"seldonio/tfserving-proxy:0.2"` |  |
