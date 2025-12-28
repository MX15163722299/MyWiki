你的错误提示 **“Your index contains uncommitted changes”**，说明本地有未提交的更改，Git 不允许你直接执行 `git pull --rebase`。你可以选择 **提交更改** 或 **暂存更改（stash）** 后再拉取远程代码。

------

## **✅ 解决方案**

### **方法 1：提交本地更改（推荐）**

如果你希望保留当前更改并推送到远程：

```sh
git add .
git commit -m "Save local changes before pulling"
git pull --rebase origin master
git push origin master
```

这样你的更改会被保留，并且 Git 允许你同步远程代码。

------

### **方法 2：暂存（stash）本地更改**

如果你不想立即提交，可以使用 `git stash` 将更改存入 Git 的临时存储区：

```sh
git stash
git pull --rebase origin master
git stash pop  # 恢复你的更改
```

然后再执行：

```sh
git add .
git commit -m "Apply local changes after pull"
git push origin master
```

------

### **方法 3：强制拉取（⚠️ 小心使用）**

如果你确定本地更改不重要，可以使用 `--hard` 选项放弃所有未提交的更改：

```sh
git reset --hard
git pull origin master
git push origin master
```

**⚠️ 警告**：`git reset --hard` **会丢失所有未提交的更改**，请谨慎使用！

------

## **🚀 结论**

- **如果本地更改重要** ➝ `git add . && git commit -m "message"` 再 `git pull --rebase`
- **如果只是临时更改** ➝ `git stash` 再 `git pull --rebase`
- **如果要放弃更改** ➝ `git reset --hard` 再 `git pull`

你可以试试哪种方法适合你的情况，如果还有问题可以贴一下 `git status`，我帮你具体分析！ 😊