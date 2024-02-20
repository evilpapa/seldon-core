# 为 Seldon Core 贡献

_在打开 PR 前_ 考虑：

- 变更是否足够重要并准备完毕以让社区花费时间经理来进行 review？
- 你有没有搜索已经存在或相关的讨论或者PR？
- 提议的变更是否得到了明确的解释和目的？

当您贡献代码时，您确认该贡献是您的原创作品，并且您
根据项目的开源许可证将作品授权给该项目。不管你是否
通过 PR 请求、电子邮件或
其他表示您同意根据项目的开源许可证对材料进行许可，并且
保证你有这样做的合法权限。

## 发布书名

我们管理发行说明的过程是以 Kubernetes 项目如何处理它们为模型的。
该过程可分为两个独立阶段：

- 在 **PR 创建时** 为每个 PR 增加说明。
- **发布时** 在发布之前编译所有PR注释。

### 为每个 PR 增加说明

当 PR 创建，一个 [Prow / Lighthouse
插件](https://prow.k8s.io/command-help#release_note_none) 会检查 PR 体中是否存在一个相应的 `release-note` 区块如：

````md
```release-note
Some public-facing release note.
```
````

如果没有，PR 会被打标为 `do-not-merge/release-note-label-needed`。
请注意，为了加快速度，[默认的 PR 模板](https://github.com/SeldonIO/seldon-core/blob/master/.github/PULL_REQUEST_TEMPLATE.md)
会为你创建一个空的 `release-notes` 块。
对于不需要面向公众的发布说明的 PR (e.g. 继承测试的修复程序)，你可以使用 `/release-note-none` Prow 命令。

#### 约定

我们可以使用许多约定，以便更改更具语义。
这些主要基于关键字，这些关键字将影响发布说明的显示方式。

- 使用 `Added`、`Changed`、`Fixed`、`Removed` 或者 `Deprecated` 单词来描述 PR 内容。
  例如：
  
  ````md
  ```release-note
  Added metadata support to Python wrapper
  ```
  ````

- 使用表达式 `Action required` 描述中断更改。
  For example:

  ````md
  ```release-note
  Action required: The helm value `createResources` has been renamed
  `managerCreateResources`.
  ```
  ````

### 发布前编译所有PR注释

发布时，有一个 [release-notes命令](https://github.com/kubernetes/release/blob/master/cmd/release-notes/README.md)
会在 2 个特定标签之间的所有 PR 上爬取行提取发行说明块（例如 `v1.1.0` 到 `v1.2.0`）。
然后可以使用这些块来生成最终发行说明。

## 编码约定

我们使用 [pre-commit](https://pre-commit.com/) 处理一些 Git hooks，它能确保
所有基线代码的修改能遵循 Seldon 代码约定。
推荐在任何修改代码前进行设置。
如果代码不符合存储库中每种语言的样式指南，那么接下来的额外检查将停止构建。

参考 [官方说明](https://pre-commit.com/#install) 进行安装。
一旦安装完成，请运行：

```console
$ pre-commit install
```

这将会读取 `.pre-commit-config.yaml` 的 hook 定义并照着安装到本地仓库。

### Java

Java 代码格式遵循 [Google's Java 风格指南](https://google.github.io/styleguide/javaguide.html)。
为了确保代码库保持一致，我们使用
[checkstyle](https://github.com/checkstyle/checkstyle) 作为 `mvn validate` 生命周期的一部分。

要在本地编辑器整合，你可以参考 [本地配置 checkstyle](https://checkstyle.org/beginning_development.html) 的官方说明并
[设置 google-java-format](https://github.com/google/google-java-format#using-the-formatter)。

### Python

Python 代码格式遵循 [black](https://github.com/psf/black)，一个很重的格式化程序。

要在本地编辑器整合，你可以参考 [设置 black](https://github.com/psf/black#editor-integration) 的官方说明。

## Tests

无论您正在处理哪个包，我们都将主要任务抽象为 `Makefile`。
因为，为了执行测试，你需要这么做：

```bash
$ make test
```

### Python

我们使用 [pytest](https://docs.pytest.org/en/latest/) 作为主要的执行器。
然而，为了确保测试能在最终用户通过 `pip` 及 pypi.org 下载的相同版本的包上运行，我们在其上使用 [tox](https://tox.readthedocs.io/en/latest/)。
安装两者（亦或者其他以来的插件），使用：

```bash
$ make install_dev
```

我们可以使用 `tox` 在不同的隔离环境中运行整个测试套件。
您可以在文件中看到我们当前使用的不同
[setup.cfg](https://github.com/SeldonIO/seldon-core/blob/master/python/setup.cfg)
文件。
你可以在所有环境中使用 `make test` [mentioned above](#Tests) 运行测试。
或者，如果您想传递任何额外的参数，你也可以直接运行 `tox`：

```bash
$ tox
```

一个 `tox` 警告是，随着环境数量的增长，完成测试所需的时间也在增加。
作为解决方案，在本地环境推荐你直接运行 `pytest`：
你可以直接执行：

```bash
$ pytest
```

### 端到端测试

作为 Seldon Core 的测试套件，我们也支持端到端测试。
这些使用 [Kind](https://github.com/kubernetes-sigs/kind) 生成了一个实际的Kubernetes集群并
部署不同的 `SeldonDeployment` 及资源。

你可以通过 [专门的文档](https://docs.seldon.io/projects/seldon-core/en/latest/developer/e2e.html) 学习如何运行并添加测试用例。
