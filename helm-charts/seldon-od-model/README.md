# seldon-od-model

![Version: 0.2.0](https://img.shields.io/static/v1?label=Version&message=0.2.0&color=informational&style=flat-square)

Chart to deploy an outlier detector as a single Seldon model.

## Usage

使用本chart，首先要添加 `seldonio` Helm 仓库：

```bash
helm repo add seldonio https://storage.googleapis.com/seldon-charts
helm repo update
```

一旦完成，你可以按照如下使用预估图模板：

```bash
helm template $MY_MODEL_NAME seldonio/seldon-od-model --namespace $MODELS_NAMESPACE
```

注意你也可以直接部署预估图到集群中：
使用：

```bash
helm install $MY_MODEL_NAME seldonio/seldon-od-model --namespace $MODELS_NAMESPACE
```

**主页:** <https://github.com/SeldonIO/seldon-core>

## 源码

* <https://github.com/SeldonIO/seldon-core>
* <https://github.com/SeldonIO/seldon-core/tree/master/helm-charts/seldon-od-model>

## 设置值

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| engine.env.SELDON_LOG_MESSAGES_EXTERNALLY | bool | `false` |  |
| engine.env.SELDON_LOG_MESSAGE_TYPE | string | `"seldon.message.pair"` |  |
| engine.env.SELDON_LOG_REQUESTS | bool | `false` |  |
| engine.env.SELDON_LOG_RESPONSES | bool | `false` |  |
| engine.resources.requests.cpu | string | `"0.1"` |  |
| model.isolationforest.image.name | string | `"seldonio/outlier-if-model:0.1"` |  |
| model.isolationforest.load_path | string | `"./models/"` |  |
| model.isolationforest.model_name | string | `"if"` |  |
| model.isolationforest.threshold | int | `0` |  |
| model.mahalanobis.image.name | string | `"seldonio/outlier-mahalanobis-model:0.1"` |  |
| model.mahalanobis.max_n | int | `-1` |  |
| model.mahalanobis.n_components | int | `3` |  |
| model.mahalanobis.n_stdev | int | `3` |  |
| model.mahalanobis.start_clip | int | `50` |  |
| model.mahalanobis.threshold | int | `25` |  |
| model.name | string | `"outlier-detector"` |  |
| model.parameterTypes.load_path | string | `"STRING"` |  |
| model.parameterTypes.max_n | string | `"INT"` |  |
| model.parameterTypes.model_name | string | `"STRING"` |  |
| model.parameterTypes.n_components | string | `"INT"` |  |
| model.parameterTypes.n_stdev | string | `"FLOAT"` |  |
| model.parameterTypes.reservoir_size | string | `"INT"` |  |
| model.parameterTypes.start_clip | string | `"INT"` |  |
| model.parameterTypes.threshold | string | `"FLOAT"` |  |
| model.seq2seq.image.name | string | `"seldonio/outlier-s2s-lstm-model:0.1"` |  |
| model.seq2seq.load_path | string | `"./models/"` |  |
| model.seq2seq.model_name | string | `"seq2seq"` |  |
| model.seq2seq.reservoir_size | int | `50000` |  |
| model.seq2seq.threshold | float | `0.003` |  |
| model.type | string | `"vae"` | Type of outlier detector. Valid values are: `vae`, `mahalanobis`, `seq2seq` and `isolationforest`. |
| model.vae.image.name | string | `"seldonio/outlier-vae-model:0.1"` |  |
| model.vae.load_path | string | `"./models/"` |  |
| model.vae.model_name | string | `"vae"` |  |
| model.vae.reservoir_size | int | `50000` |  |
| model.vae.threshold | int | `10` |  |
| name | string | `"seldon-od-model"` |  |
| predictorLabels.fluentd | string | `"true"` |  |
| predictorLabels.version | string | `"v1"` |  |
| replicas | int | `1` |  |
| sdepLabels.app | string | `"seldon"` |  |
