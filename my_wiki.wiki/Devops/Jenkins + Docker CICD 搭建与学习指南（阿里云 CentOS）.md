# **Jenkins + Docker CI/CD 搭建与学习指南（阿里云 CentOS）**

本教程适用于 **阿里云 CentOS 服务器**，搭建 **Jenkins + Docker** 自动化 **代码构建、镜像制作、容器部署**，实现完整的 **CI/CD 流程**。

------

## **📌 1. 环境准备**

### **1.1 服务器信息**

- **系统**：CentOS 7 / 8（推荐 64 位）
- **服务器**：阿里云 ECS
- **最低配置**：2 核 CPU，4G 内存，50G 磁盘
- **公网 IP**：建议绑定（方便访问）

### **1.2 需要安装的软件**

| **软件**           | **版本**   | **用途**         |
| ------------------ | ---------- | ---------------- |
| **Jenkins**        | LTS 最新版 | CI/CD 自动化工具 |
| **Git**            | 最新版     | 代码管理         |
| **Java (JDK 17)**  | OpenJDK 17 | 运行 Jenkins     |
| **Docker**         | 最新版     | 容器化部署       |
| **Docker Compose** | 最新版     | 容器编排         |
| **Nginx (可选)**   | 最新版     | 代理 Jenkins     |

------

## **📌 2. 安装 Jenkins**

### **2.1 更新系统并安装必要依赖**

```bash
sudo yum update -y
sudo yum install -y wget git vim
```

### **2.2 安装 Java**

```bash
sudo yum install -y java-17-openjdk-devel
java -version  # 验证安装
```

### **2.3 添加 Jenkins 源**

```bash
wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
```

### **2.4 安装 Jenkins**

```bash
sudo yum install -y jenkins
```

### **2.5 启动 Jenkins**

```bash
sudo systemctl enable --now jenkins
sudo systemctl status jenkins
```

### **2.6 打开 Jenkins Web 界面**

- 浏览器访问 `http://<服务器公网IP>:8080`
- 获取 Jenkins 初始密码：

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

- 输入密码，按照引导完成 Jenkins 初始化配置（推荐安装 **建议插件**）

------

## **📌 3. 安装 Docker**

### **3.1 卸载旧版本**

```bash
sudo yum remove -y docker docker-common docker-selinux \
    docker-engine docker-client docker-cli
```

### **3.2 安装 Docker**

```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

### **3.3 启动 Docker 并设置开机自启**

```bash
sudo systemctl enable --now docker
docker --version  # 验证 Docker 安装
```

### **3.4 让 Jenkins 运行 Docker**

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

------

## **📌 4. 配置 Jenkins 使其支持 Docker**

### **4.1 安装 Docker 插件**

- 进入 Jenkins **插件管理**
- 搜索 `Docker Pipeline` 插件并安装
- 安装 `Pipeline` 插件（用于 CI/CD 流水线）

### **4.2 配置 Jenkins 访问 Docker**

- 进入 `Jenkins -> 系统管理 -> 系统配置`

- 在 

  ```
  Docker
  ```

   配置中，设置 

  ```
  Docker Host
  ```

   为：

  ```
  unix:///var/run/docker.sock
  ```

- 保存配置

------

## **📌 5. 创建 CI/CD 流水线**

### **5.1 创建 Git 仓库**

在 GitHub / Gitee 创建一个仓库，例如：

```
https://github.com/yourname/jenkins-docker-ci
```

并添加 `Dockerfile`：

```dockerfile
# 基于 Python 运行环境
FROM python:3.9
WORKDIR /app
COPY . .
CMD ["python3", "app.py"]
```

创建 `app.py`：

```python
print("Hello, Jenkins + Docker CI/CD!")
```

------

### **5.2 在 Jenkins 创建 Pipeline**

1. **进入 Jenkins** → 新建 **流水线项目**
2. 配置 Git 仓库
   - 选择 `Git`，输入你的仓库地址
   - 如果是私有仓库，需要配置 SSH Key 或凭据
3. **添加 Pipeline 脚本** 在 **Pipeline 配置** 中输入：

```groovy
pipeline {
    agent any
    stages {
        stage('拉取代码') {
            steps {
                git 'https://github.com/yourname/jenkins-docker-ci.git'
            }
        }
        stage('构建 Docker 镜像') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }
        stage('运行容器') {
            steps {
                sh 'docker run -d --name myapp -p 5000:5000 myapp:latest'
            }
        }
    }
}
```

1. **保存并运行 Jenkins 任务**

------

## **📌 6. 验证 CI/CD**

1. 执行 Jenkins Pipeline
   - 在 Jenkins 控制台点击 `立即构建`
   - 观察日志，确认 Docker 镜像构建成功
   - 运行 `docker ps` 确认容器运行
2. 访问应用
   - 在浏览器打开 `http://<服务器公网IP>:5000`
   - 应该能看到 `Hello, Jenkins + Docker CI/CD!`

------

## **📌 7. CI/CD 进阶**

✅ **推送 Docker 镜像到阿里云容器镜像服务（ACR）**

```bash
docker tag myapp:latest registry.cn-hangzhou.aliyuncs.com/yourname/myapp:latest
docker login --username=你的用户名 registry.cn-hangzhou.aliyuncs.com
docker push registry.cn-hangzhou.aliyuncs.com/yourname/myapp:latest
```

✅ **使用 Docker Compose 进行多容器部署**

- 创建 `docker-compose.yml`

```yaml
version: '3'
services:
  web:
    image: myapp:latest
    ports:
      - "5000:5000"
```

- 启动

```bash
docker-compose up -d
```

✅ **结合 Nginx 代理**

- 使用 Nginx 作为 Jenkins 反向代理，提供更安全的访问

✅ **Webhook 自动触发 CI/CD**

- 在 GitHub / Gitee 配置 Webhook，使 Jenkins 在代码提交后自动触发构建

------

## **📌 8. 总结**

✅ **Jenkins 安装 & 配置** ✅ **Docker 安装 & 权限配置** ✅ **Jenkins + Docker 构建 CI/CD** ✅ **Docker 容器运行** ✅ **进阶：阿里云 ACR、Docker Compose、Nginx**

这样，你就完成了 **Jenkins + Docker CI/CD** 流水线，从 **代码提交 → 构建 → 部署** 实现全流程自动化！ 🚀

