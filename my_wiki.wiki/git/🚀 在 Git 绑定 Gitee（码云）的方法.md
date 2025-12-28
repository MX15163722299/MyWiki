## **🚀 在 Git 绑定 Gitee（码云）的方法**

在 Git 绑定 Gitee，可以使用 **SSH 方式** 或 **HTTPS 方式** 进行认证。
 **推荐使用 SSH 方式**，因为它更安全，并且可以避免频繁输入密码。

------

## **✅ 方法 1：SSH 方式（推荐）**

### **1️⃣ 生成 SSH 密钥**

如果你本地还没有 SSH 密钥，运行以下命令：

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

一路回车，不要设置密码。
 SSH 密钥默认会存储在：

```
~/.ssh/id_rsa      # 私钥（不能泄露）
~/.ssh/id_rsa.pub  # 公钥（需要上传到 Gitee）
```

### **2️⃣ 复制 SSH 公钥**

运行以下命令查看公钥：

```bash
cat ~/.ssh/id_rsa.pub
```

复制输出的 **公钥内容**（一长串 `ssh-rsa ...`）。

### **3️⃣ 添加 SSH 公钥到 Gitee**

- 登录 Gitee（码云）：[https://gitee.com](https://gitee.com/)
- 进入 **个人设置 → SSH公钥**：[Gitee SSH 配置](https://gitee.com/profile/sshkeys)
- **新建 SSH Key**，粘贴刚才复制的 `id_rsa.pub` 内容，保存。

### **4️⃣ 测试 SSH 连接**

运行以下命令：

```bash
ssh -T git@gitee.com
```

如果成功，会看到：

```bash
Hi username! You've successfully authenticated, but Gitee does not provide shell access.
```

表示 SSH 配置成功！🎉

### **5️⃣ 使用 SSH 方式克隆 Gitee 仓库**

```bash
git clone git@gitee.com:your_username/your_repository.git
```

### **6️⃣ 修改已有项目为 SSH**

如果之前是 HTTPS 地址，修改为 SSH：

```bash
git remote set-url origin git@gitee.com:your_username/your_repository.git
```

------

## **✅ 方法 2：HTTPS 方式（适用于受限环境）**

如果不想使用 SSH，也可以用 **HTTPS + 个人访问令牌（Token）** 进行认证。

### **1️⃣ 获取 Gitee 个人访问令牌**

- **进入 Gitee 个人设置 → 安全设置 → 个人访问令牌**：[Gitee Token 设置](https://gitee.com/profile/personal_access_tokens)
- **创建 Token**，勾选 **repo（仓库）** 权限，生成后复制 Token。

### **2️⃣ 克隆 Gitee 仓库（HTTPS）**

使用 Token 而不是密码：

```bash
git clone https://your_username:your_token@gitee.com/your_username/your_repository.git
```

如果你使用 **Git 认证管理器**，可以直接：

```bash
git clone https://gitee.com/your_username/your_repository.git
```

然后在提示输入密码时，填入 **Token**（而不是 Gitee 密码）。

### **3️⃣ 修改已有项目为 HTTPS**

如果之前是 SSH 地址，改为 HTTPS：

```bash
git remote set-url origin https://gitee.com/your_username/your_repository.git
```

------

## **🚀 总结**

| 方式            | **优点**                               | **缺点**                            |
| --------------- | -------------------------------------- | ----------------------------------- |
| **SSH（推荐）** | 安全性高，操作方便，不需要频繁输入密码 | 需要手动配置 SSH 密钥               |
| **HTTPS**       | 适用于受限网络或企业防火墙环境         | 每次 `push/pull` 可能需要输入 Token |

✅ **推荐开发者使用 SSH，企业或受限环境可选择 HTTPS + Token！** 🚀