## **🚀 HTTPS vs. SSH：Git 远程仓库地址的区别**

在 `git clone` 或 `git remote add` 时，我们通常会使用 **HTTPS** 或 **SSH** 地址。两者的主要区别如下：

| **对比项**   | **HTTPS**                                                    | **SSH**                        |
| ------------ | ------------------------------------------------------------ | ------------------------------ |
| **地址格式** | `https://github.com/user/repo.git`                           | `git@github.com:user/repo.git` |
| **身份验证** | 每次操作都需要输入用户名和密码（或使用 Personal Access Token 代替密码） | 通过 SSH 密钥进行认证，免密码  |
| **访问权限** | 适用于公开和私有仓库                                         | 适用于私有仓库和组织仓库       |
| **安全性**   | 需要 Personal Access Token（PAT）来代替密码（GitHub 不再支持密码） | 需要设置 SSH 公钥，更安全      |
| **使用场景** | 适用于临时访问、CI/CD 或无法配置 SSH 的环境                  | 适用于长期开发，推荐开发者使用 |

------

## **🎯 详细对比**

### **1️⃣ HTTPS 地址**

#### ✅ **优点**

- 不需要额外配置，直接使用浏览器或 Git 命令行 `clone` 仓库。
- 适用于公司或受限网络环境，很多企业防火墙会阻止 SSH 端口（22）。
- 适用于 **CI/CD 自动化**，可使用 **Personal Access Token (PAT)** 进行认证。

#### ❌ **缺点**

- 每次 `push` 或 `pull` 需要输入用户名和密码（或者 Token）。
- GitHub、Gitee 等平台已**不支持直接使用密码**，必须用 **Personal Access Token（PAT）**。

#### 🌟 **示例**

```bash
git clone https://github.com/user/repo.git
```

如果是私有仓库，GitHub 要求用 **Token 代替密码**：

```bash
git clone https://<USERNAME>:<TOKEN>@github.com/user/repo.git
```

------

### **2️⃣ SSH 地址**

#### ✅ **优点**

- 认证方式更安全，只需要设置 SSH **公钥**，不需要每次输入密码。
- 适合长期开发，Git 操作更加顺畅（不需要每次输入密码）。
- 推荐给 **开发人员** 使用。

#### ❌ **缺点**

- 需要**手动配置 SSH 密钥**。
- 一些公司或防火墙会屏蔽 **SSH（22 端口）**，导致无法使用 SSH 连接 Git 服务器。

#### 🌟 **示例**

```bash
git clone git@github.com:user/repo.git
```

------

## **🚀 如何选择？**

✅ **使用 HTTPS**

- 如果你不想配置 SSH 密钥，或者只想临时 `clone` 仓库。
- 如果你在公司或受限网络环境（防火墙可能禁用 SSH）。
- 如果你在 **CI/CD** 或 **脚本** 中使用 Git，HTTPS 搭配 **Personal Access Token（PAT）** 认证更方便。

✅ **使用 SSH**

- 如果你是 **开发人员**，并且希望**长期**使用 Git（推荐）。
- 如果你不想每次 `push/pull` 时都输入密码。
- 如果你需要访问私有仓库，而不想用 HTTPS Token 认证。

------

## **🎯 如何配置 SSH？**

如果你想使用 **SSH** 连接 GitHub 或 Gitee，可以按照以下步骤操作：

### **1️⃣ 生成 SSH 密钥**

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

一路回车，密钥会存储在：

```bash
~/.ssh/id_rsa      # 私钥（不要泄露）
~/.ssh/id_rsa.pub  # 公钥（可以上传）
```

### **2️⃣ 添加公钥到 GitHub/Gitee**

查看 SSH 公钥：

```bash
cat ~/.ssh/id_rsa.pub
```

然后：

- **GitHub**：进入 [GitHub SSH 设置](https://github.com/settings/keys)，添加公钥。
- **Gitee**：进入 [Gitee SSH 设置](https://gitee.com/profile/sshkeys)，添加公钥。

### **3️⃣ 测试 SSH 连接**

```bash
ssh -T git@github.com
```

如果配置成功，会看到：

```bash
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

### **4️⃣ 配置 Git 使用 SSH**

如果你原来是 HTTPS，改为 SSH：

```bash
git remote set-url origin git@github.com:user/repo.git
```

------

## **🎯 总结**

| **场景**                | **推荐地址**          |
| ----------------------- | --------------------- |
| **临时 clone 代码**     | ✅ HTTPS               |
| **开发人员长期使用**    | ✅ SSH                 |
| **CI/CD 或脚本**        | ✅ HTTPS（搭配 Token） |
| **防火墙限制 SSH 端口** | ✅ HTTPS               |
| **企业内部 Git 服务器** | ✅ 可能更推荐 SSH      |

🔹 **开发者推荐 SSH，企业或 CI/CD 可使用 HTTPS 搭配 Token！** 🚀