## **从 Git 仓库 Clone 下来后，如何使用虚拟环境运行 Python 代码？**

当你从 Git 仓库 `clone` 下来一个 Python 项目后，通常需要创建一个**虚拟环境**来管理 Python 依赖，确保环境一致性。以下是完整的操作步骤：

------

### **🚀 步骤 1：Clone Git 仓库**

首先，使用 `git clone` 命令从远程仓库拉取代码：

```bash
git clone <your-repo-url>
cd <your-project-folder>
```

示例：

```bash
git clone https://github.com/example/my-python-project.git
cd my-python-project
```

------

### **🚀 步骤 2：创建虚拟环境**

在项目目录下，运行以下命令创建一个虚拟环境：

#### **🔹 使用 venv（推荐）**

Python 内置的 `venv` 模块可以创建虚拟环境：

```bash
python -m venv venv
```

- `venv` 是虚拟环境的目录名，你可以改成 `env`、`.venv` 之类的名称。

#### **🔹 使用 virtualenv（如果安装了）**

```bash
virtualenv venv
```

如果 `virtualenv` 没有安装，可以先运行：

```bash
pip install virtualenv
```

------

### **🚀 步骤 3：激活虚拟环境**

激活虚拟环境，使其生效：

#### **🔹 Windows**

```bash
venv\Scripts\activate
```

如果 `PowerShell` 提示权限错误，可以运行：

```powershell
Set-ExecutionPolicy Unrestricted -Scope Process
venv\Scripts\activate
```

#### **🔹 macOS / Linux**

```bash
source venv/bin/activate
```

成功激活后，你的终端前面应该会出现 `(venv)`，例如：

```bash
(venv) user@computer:~/my-python-project$
```

------

### **🚀 步骤 4：安装项目依赖**

通常，项目的依赖会在 `requirements.txt` 或 `pyproject.toml` 里。

#### **🔹 如果有 `requirements.txt`**

```bash
pip install -r requirements.txt
```

#### **🔹 如果有 `pyproject.toml`（Poetry 方式）**

如果项目使用 **Poetry**，你可以运行：

```bash
poetry install
```

如果没有安装 `poetry`，可以先安装：

```bash
pip install poetry
```

------

### **🚀 步骤 5：运行代码**

现在你可以在虚拟环境中运行 Python 代码。例如：

```bash
python main.py
```

或者，如果是 **FastAPI** 项目，可能会用 `uvicorn` 运行：

```bash
uvicorn main:app --reload
```

------

### **🚀 步骤 6：退出虚拟环境**

如果你想退出虚拟环境，可以运行：

```bash
deactivate
```

------

## **🎯 总结**

| **步骤**                        | **命令**                          |
| ------------------------------- | --------------------------------- |
| **克隆项目**                    | `git clone <repo-url>`            |
| **进入项目目录**                | `cd my-python-project`            |
| **创建虚拟环境**                | `python -m venv venv`             |
| **激活虚拟环境（Windows）**     | `venv\Scripts\activate`           |
| **激活虚拟环境（macOS/Linux）** | `source venv/bin/activate`        |
| **安装依赖**                    | `pip install -r requirements.txt` |
| **运行代码**                    | `python main.py`                  |
| **退出虚拟环境**                | `deactivate`                      |

------

按照这个流程，你就可以在本地正确运行 Python 代码了！🚀