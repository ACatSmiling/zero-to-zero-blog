> *`Author: ACatSmiling`*
>
> *`Since: 2025-02-09`*

**`Miniconda`**：Python 环境管理工具。

```powershell
# 查看 python 环境列表
$ conda env list

# 创建新的 python 环境
$ conda create -n <env-name> python=<version>

# 激活指定的 python 环境
$ conda activate <env-name>

# 删除指定的 python 环境
$ conda remove --name <env-name> --all
```

**`Poetry`**：Python 包管理工具。

```powershell
# 已有 pyproject.toml，poetry 安装依赖
$ poetry install

# 查看已经安装的依赖
$ poetry show

# 或者激活虚拟环境查看
$ poetry shell
$ pip list

# 或者通过 conda 激活对应的 python 环境，然后查看
```

- 说明：`poetry show` 命令如果显示模块为红色，通常表示这些模块存在问题，可能是未正确安装、版本冲突或环境配置错误。此时，可以使用`poetry check`命令，检查是否存在版本冲突，如果不存在冲突，通过`poetry install <model-name>`也安装不了，则使用`pip install <model-name>`尝试安装。

补充：VS Code 指定 Python 环境。

- 快捷键`Ctrl+Shift+P`，输入`Python: Select Interpreter`，在弹出的列表中选择希望使用的 Python 解释器。
- 如果已经安装了 Python 扩展（如 Microsoft 的 Python Extension），并且正确配置了 Python 解释器，可以在状态栏中看到 Python 版本。