# seldon-mab

![Version: 0.2.0](https://img.shields.io/static/v1?label=Version&message=0.2.0&color=informational&style=flat-square)

部署一个多臂赌博机路由器的 chart ，用于在两个 Seldon 部署上，将流量发送到表现最佳的模型。
你会利用到 `predict` 及 `send_feedback` 两个 API 方法。

## Usage

使用本chart，首先要添加 `seldonio` Helm 仓库：

```bash
helm repo add seldonio https://storage.googleapis.com/seldon-charts
helm repo update
```

一旦完成，你可以按照如下使用预估图模板：

```bash
helm template $MY_MODEL_NAME seldonio/seldon-mab --namespace $MODELS_NAMESPACE
```

注意你也可以直接部署预估图到集群中：
使用：

```bash
helm install $MY_MODEL_NAME seldonio/seldon-mab --namespace $MODELS_NAMESPACE
```

**主页:** <https://github.com/SeldonIO/seldon-core>

## 源码

* <https://github.com/SeldonIO/seldon-core>
* <https://github.com/SeldonIO/seldon-core/tree/master/helm-charts/seldon-mab>

## 设置值

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| engine.env.SELDON_LOG_MESSAGES_EXTERNALLY | bool | `false` |  |
| engine.env.SELDON_LOG_MESSAGE_TYPE | string | `"seldon.message.pair"` |  |
| engine.env.SELDON_LOG_REQUESTS | bool | `false` |  |
| engine.env.SELDON_LOG_RESPONSES | bool | `false` |  |
| engine.resources.requests.cpu | string | `"0.1"` |  |
| mab.branches | int | `2` |  |
| mab.epsilon | float | `0.2` |  |
| mab.image.name | string | `"seldonio/mab_epsilon_greedy"` |  |
| mab.image.version | string | `"1.14.0"` |  |
| mab.name | string | `"eg-router"` |  |
| mab.verbose | int | `1` |  |
| modela.image.name | string | `"seldonio/mock_classifier"` |  |
| modela.image.version | string | `"1.14.0"` |  |
| modela.name | string | `"classifier-1"` |  |
| modelb.image.name | string | `"seldonio/mock_classifier"` |  |
| modelb.image.version | string | `"1.14.0"` |  |
| modelb.name | string | `"classifier-2"` |  |
| predictor.name | string | `"default"` |  |
| predictorLabels.fluentd | string | `"true"` |  |
| predictorLabels.version | string | `"1.14.0"` |  |
| replicas | int | `1` |  |
| sdepLabels.app | string | `"seldon"` |  |
