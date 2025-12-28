![image-20250312133051363](C:\Users\Y7000\AppData\Roaming\Typora\typora-user-images\image-20250312133051363.png)

### **将 `dev` 分支合并到本地 `master` 分支的步骤**

如果你想将 **`dev` 分支** 合并到本地 **`master` 分支**，可以按照以下步骤操作。

------

### **✅ 步骤 1：切换到 `master` 分支**

确保你在 `master` 分支：

```bash
git checkout master
```

如果本地没有 `master` 分支，可以创建：

```bash
git checkout -b master
```

------

### **✅ 步骤 2：拉取最新 `master` 代码**

确保你的 `master` 分支是最新的：

```bash
git pull origin master
```

------

### **✅ 步骤 3：合并 `dev` 分支到 `master`**

```bash
git merge dev
```

如果 `dev` 分支的代码比 `master` 版本新，并且两者没有冲突，会直接合并。

------

### **✅ 步骤 4：解决合并冲突（如有）**

如果 Git 提示有冲突：

1. 运行 

   ```
   git status
   ```

    查看冲突文件：

   ```bash
   git status
   ```

2. 打开冲突文件，手动修改冲突部分。

3. 修改完后，标记冲突已解决：

   ```bash
   git add .
   ```

4. 提交合并：

   ```bash
   git commit -m "Merge dev into master"
   ```

------

### **✅ 步骤 5：推送 `master` 分支到远程**

```bash
git push origin master
```

------

## **🎯 另一种方式：使用 `rebase`**

如果你希望 `master` 分支保持更清晰的提交历史，可以使用 `rebase`：

```bash
git checkout master
git pull origin master
git rebase dev
```

但如果有冲突，`rebase` 需要你手动解决后执行：

```bash
git rebase --continue
```

------

## **🔹 总结**

| **操作**               | **命令**                                                     |
| ---------------------- | ------------------------------------------------------------ |
| 切换到 `master`        | `git checkout master`                                        |
| 更新 `master`          | `git pull origin master`                                     |
| 合并 `dev` 到 `master` | `git merge dev`                                              |
| 解决冲突               | `git status` → 手动修改 → `git add .` → `git commit -m "Resolve conflicts"` |
| 推送 `master`          | `git push origin master`                                     |

这样，你的 `dev` 分支代码就成功合并到 `master` 了！🚀