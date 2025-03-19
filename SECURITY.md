# 安全政策

本文档概述了 Seldon Core 的安全政策。

Seldon Core 旨在遵循以下两项政策：

* 通过升级版本解决项目依赖项中的 CVE
* 通过执行推荐的升级解决 Docker 镜像中的 CVE

# 安全扫描

在每次发布时，我们都会进行安全扫描。扫描包括依赖项和 Docker 镜像扫描。

您可以在此处找到用于扫描的[确切命令](https://github.com/SeldonIO/seldon-core/blob/master/.github/workflows/security_tests.yml)，以及每次运行生成的[报告](https://github.com/SeldonIO/seldon-core/actions/workflows/security_tests.yml)。

## 支持的版本

我们使用语义化版本管理。我们发布安全补丁作为最新的主版本.次版本的`补丁版本`。

## 报告漏洞

如果您发现漏洞，如果是公开的 CVE，最好的报告方式是打开一个类型为“bug”的问题，然后可以在票据上讨论下一步（即更新库，联系第三方项目等）。

