# Seldon Core Helm Charts

Helm charts 发布在我们的官方仓库。

## 核心 Charts

以下是安装 Seldon Core 的核心 charts。

 * [seldon-core-operator](https://docs.seldon.io/projects/seldon-core/en/latest/charts/seldon-core-operator.html)
   * 安装 Seldon Core CRD 及 Controller 的主要 helm chart
 * [seldon-core-analytics](https://docs.seldon.io/projects/seldon-core/en/latest/charts/seldon-core-analytics.html)
   * 演示 Seldon Core 的示例 Prometheus 和 Grafana 设置，包括展示 Grafana 仪表板。


## Seldon Core 预估图模板

一组 chart，为使用 Seldon Core 创建特定推理图的示例模板提供支持。

 * [seldon-single-model](https://docs.seldon.io/projects/seldon-core/en/latest/charts/seldon-single-model.html)
   * 继承存储卷的单模型服务。
 * [seldon-abtest](https://docs.seldon.io/projects/seldon-core/en/latest/charts/seldon-abtest.html)
   * 继承AB 测试的两个模型服务。
 * [seldon-mab](https://docs.seldon.io/projects/seldon-core/en/latest/charts/seldon-mab.html)
   * 在两个模型之间提供多臂赌博机服务。
 * [seldon-od-model](https://docs.seldon.io/projects/seldon-core/en/latest/charts/seldon-od-model.html) and [seldon-od-transformer](https://docs.seldon.io/projects/seldon-core/en/latest/charts/seldon-od-transformer.html)
   * 将以下异常检测组件之一作为模型或转换器提供服务：
     * [Isolation Forest](https://github.com/SeldonIO/seldon-core/tree/master/components/outlier-detection/isolation-forest)
     * [Variational Auto-Encoder](https://github.com/SeldonIO/seldon-core/tree/master/components/outlier-detection/vae)
     * [Sequence-to-Sequence-LSTM](https://github.com/SeldonIO/seldon-core/tree/master/components/outlier-detection/seq2seq-lstm)
     * [Mahalanobis Distance](https://github.com/SeldonIO/seldon-core/tree/master/components/outlier-detection/mahalanobis)

参考[这里]使用如上示例(https://github.com/SeldonIO/seldon-core/tree/master/notebooks/helm_examples.ipynb)。

## 杂项

 * [seldon-core-loadtesting](https://docs.seldon.io/projects/seldon-core/en/latest/charts/seldon-core-loadtesting.html)
   * 负载测试工具

## 文档

为了生成 Helm charts 文档，我们使用 [`helm-docs`](https://github.com/norwoodj/helm-docs)。
该工具会读取 `Chart.yaml` 及 `values.yaml` 包含的元数据信息来生成 `README.md` 页。

### 本地生成文档

你可以使用 `Makefile` 的 `install` 选项来安装 `helm-docs` 最新版本。

```shell
make install
```

然后，使用 `Makefile` 的 `docs` 选项：

```shell
make docs
```
