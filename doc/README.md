# Seldon Core 文档

该目录包含文档的（`.md` 和 `.rst` 文件）。
主索引页面定义在 `source/index.rst`。
Sphinx 配置和插件可以在 `source/conf.py` 找到。
文档生成通过 `make html` 命令实现。

## 要求

要构建文档，首先需要安装 Python 依赖：

```bash
make install-dev
```

### 安装 pandoc

我们使用 `pandoc` 编译 Jupyter notebooks，最简单的方式是使用 conda 安装：

`conda install -c conda-forge pandoc=1.19`

## 用法

本地构建请执行：

```bash
make html
```

编译结果保存在 `_build` 目录，并通过 `_build/html/index.html` 索引主页。

### 在线编辑

开发中，后台运行文档的变化还是非常有必要的。
通过命令运行：

```bash
make livehtml-fast
```

## Sphinx 扩展和插件

我们使用如下 Sphinx 扩展和插件来构建文档：

 * [m2r2](https://github.com/crossnox/m2r2) - 处理 `.rst` 和 `.md`
 * [sphinx.ext.napoleon](https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html) - 支持 API 文档生成提取 Numpy 风格的文档字符串
 * [sphinx_autodoc_typehints](https://github.com/agronholm/sphinx-autodoc-typehints) - 支持 API 文档生成解析类型提示
 * [sphinxcontrib.apidoc](https://github.com/sphinx-contrib/apidoc) - 在构建 API 文档期间自动运行 [sphinx-apidoc](https://www.sphinx-doc.org/en/master/man/sphinx-apidoc.html)
 * [nbsphinx](https://nbsphinx.readthedocs.io) - 解析 Jupyter notebooks 生成静态文档
 * [nbsphinx_link](https://nbsphinx-link.readthedocs.io) - 通过 `.nblink` 文件链接 Sphinx 源文件夹之外的 notebooks。

完整的插件和参数可在 `source/conf.py` 找到。

## 提示与技巧

### 链接 `doc/source` 之外的 markdown

在 `doc/source` 目录树之外引用文档并不能立即使用，但是有是一个简单的解决方法： 

1. 创建一个包含 `include` 或 `mdinclude` 指令的 `rst` 文件，比如，参考[此处](source/reference/integration_nvidia_link.rst) 链接引用到 [此处](source/reference/images.md)

        .. mdinclude:: ../../../integrations/nvidia-inference-server/README.md

2. 使用链接引用来替代包含文件

### 链接 `doc/source` 之外的 notebooks

要链接存储在 `doc` 目录之外的 notebooks 文件，你需要创建一个链接到它的 `*.nblink` 文件。

参考 [source/examples/seldon_core_setup.nblink](source/examples/seldon_core_setup.nblink) 示例。

注意有些 notebooks 可能链接了其他资源，比如图片，通常这些图片会保存在他们所在目录中（比如： `images/` 文件夹）。
这些文件也需要被引用到，这样他们在最终输出才能被正确链接。
可在 `*.nblink` 文件定义 `extra-media` 关键词来使用他们。
请参考 [source/examples/graph-metadata.nblink](source/examples/seldon_core_setup.nblink) 示例。

完整的解决方法如下所示： 

1. 创建简单的 `nblink` 文件指向 notebook：

   ```json
   {
     "path": "../../../notebooks/example-1/our-notebook.ipynb"
   }
   ```

2. （可选）添加 notebook 链接的额外资源（比如：图片）：

   ```json
   {
     "path": "../../../notebooks/example-1/our-notebook.ipynb",
     "extra-media": ["../../../notebooks/example-1/images"]
   }
   ```

3. 引用而不是加载该文件。

