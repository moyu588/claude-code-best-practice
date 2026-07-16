# 三分支 Fork 管理工作流

> **核心理念**
> 
> - `main`：只做上游同步，绝不提交自己的内容
> - `my-changes`：只提交自己的修改，永远不 rebase main
> - `release`：每次重建的"组装产物"，可以 force push

## 目录

1. [初始化（只做一次）](#一初始化只做一次)
2. [日常开发](#二日常开发)
3. [定期同步上游](#三定期同步上游)
4. [速查命令卡](#四速查命令卡)
5. [常见问题](#五常见问题)

## 一、初始化（只做一次）

### 1. 克隆你的 fork 仓库

```bash
git clone git@github.com:moyu588/claude-code-best-practice.git 
cd 你的fork仓库
```

### 2. 添加上游源仓库

```bash
git remote add upstream https://github.com/shanraisshan/claude-code-best-practice.git

# 验证，应看到 origin 和 upstream 两个远程
git remote -v
```

预期输出：

```
origin    https://github.com/你的用户名/你的fork仓库.git (fetch)
origin    https://github.com/你的用户名/你的fork仓库.git (push)
upstream  https://github.com/原作者/原仓库.git (fetch)
upstream  https://github.com/原作者/原仓库.git (push)
```

### 3. 创建 my-changes 分支

```bash
# 基于当前 main 创建
git checkout -b my-changes
git push origin my-changes
```

### 4. 创建 release 分支

```bash
git checkout -b release
git push origin release

# 回到 main
git checkout main
```

### 5. 确认分支结构

```bash
git branch -a
```

预期看到：

```
* main
  my-changes
  release
  remotes/origin/main
  remotes/origin/my-changes
  remotes/origin/release
  remotes/upstream/main
```

---

## 二、日常开发

> **规则：所有代码改动只在 `my-changes` 分支上进行，绝不碰 `main`。**

### 1. 切换到开发分支

```bash
git checkout my-changes
```

### 2. 编写代码并提交

```bash
# ... 修改文件 ...

git add .
git commit -m "feat: 描述你的改动"
```

提交前缀建议：

| 前缀          | 含义       |
| ----------- | -------- |
| `feat:`     | 新功能      |
| `fix:`      | 修复问题     |
| `chore:`    | 配置、依赖等杂项 |
| `docs:`     | 文档修改     |
| `refactor:` | 代码重构     |

### 3. 推送到 GitHub

```bash
git push origin my-changes
```

---

## 三、定期同步上游

> 建议：**上游有更新时**或**每周**执行一次，共四步。

---

### Step 1：同步上游到 main

```bash
git checkout main

# 拉取上游所有更新
git fetch upstream

# 快进合并，保持 main 是上游的纯镜像
git merge upstream/main --ff-only

# 推送到 GitHub
git push origin main
```

> ⚠️ 如果 `--ff-only` 报错，说明 `main` 上存在自己的提交，违反了工作流规则。
> 解决方式：`git reset --hard upstream/main`（会丢弃 main 上的本地提交，请确认后执行）

---

### Step 2：my-changes 什么都不做

```bash
# my-changes 上只管写自己的代码和提交
# 不需要 merge main，不需要 rebase main
# 它永远是一条独立的、干净的自定义改动线
git checkout my-changes
# ... 继续你的开发工作 ...
git push origin my-changes
```



### Step 3：重建 release 分支

> `release` 是每次重建的组装产物 = 最新上游（main）+ 你的改动（my-changes）

```bash
# 切换到 main（release 从 main 重建）
git checkout main

# 删除旧的本地 release 分支
git branch -D release

# 从 main 重新创建 release
git checkout -b release

# 将 my-changes 的所有内容合并进来
git merge my-changes --no-ff -m "release: rebuild $(date '+%Y-%m-%d')"
```

**如果出现冲突：**

```bash
# 1. 查看冲突文件
git status

# 2. 手动打开冲突文件，找到并解决冲突标记：
# <<<<<<< HEAD        ← 你的代码
# =======
# >>>>>>> main        ← 上游的代码

# 3. 解决后标记为已处理
git add <冲突文件>

# 4. 完成合并提交
git commit -m "chore: resolve conflicts with upstream"

# 5. 推送
git push origin my-changes
```

> 如果冲突复杂想放弃本次合并：
> 
> ```bash
> git merge --abort
> ```

---

### Step 4：推送 release 到 GitHub

```bash
# release 每次重建，需要强制推送
git push origin release --force-with-lease
```

> **`--force-with-lease` vs `--force` 的区别**
> 
> - `--force`：无条件覆盖远程
> - `--force-with-lease`：如果远程有你本地没有的新提交，会拒绝推送，更安全

---

## 四、速查命令卡

### 日常开发

```bash
git checkout my-changes
# ... 写代码 ...
git add .
git commit -m "feat: 你的改动描述"
git push origin my-changes
```

### 定期同步（四步走）

```bash
# Step 1：同步上游到 main
git checkout main
git fetch upstream
git merge upstream/main --ff-only
git push origin main

# Step 2：将上游更新 merge 进 my-changes
git checkout my-changes
git merge main --no-ff -m "chore: merge upstream updates into my-changes"
git push origin my-changes

# Step 3 & 4：重建并推送 release
git checkout main
git branch -D release && git checkout -b release
git merge my-changes --no-ff -m "release: rebuild $(date '+%Y-%m-%d')"
git push origin release --force-with-lease
```

---

## 五、常见问题

### Q1：`--ff-only` 合并 main 时报错怎么办？

说明你曾在 `main` 上直接提交过，执行以下命令重置：

```bash
git checkout main
git reset --hard upstream/main
git push origin main --force-with-lease
```

---

### Q2：my-changes 上的冲突太多，不知道从哪里改？

先用以下命令查看所有冲突文件列表：

```bash
git diff --name-only --diff-filter=U
```

逐个文件解决，解决一个 `git add` 一个，最后统一 `git commit`。

---

### Q3：release 分支可以直接在上面改代码吗？

**不可以。** `release` 是组装产物，下次同步时会被完全删除重建，在上面的任何修改都会丢失。
所有改动必须在 `my-changes` 上进行。

---

### Q4：如何查看 my-changes 上哪些是我自己的 commit？

```bash
# 查看 my-changes 相对于 main 多出来的 commit
git log main..my-changes --oneline
```

---

### Q5：上游仓库改名或换地址了怎么办？

```bash
# 修改 upstream 地址
git remote set-url upstream https://github.com/新地址/新仓库.git

# 验证
git remote -v
```

---

## 分支关系总览

```
upstream/main     A──B──C──D──E──F   （上游，持续更新）
                              ↓
                        fetch + merge --ff-only
                              ↓
origin/main       A──B──C──D──E──F   （纯镜像上游）
                              ↓
                          merge --no-ff
                              ↓
origin/my-changes A──B──C──D──E──F──M──X──Y   （上游历史 + 你的改动）
                                             ↓
                                         merge --no-ff
                                             ↓
origin/release    A──B──C──D──E──F──M──X──Y──R   （组装产物，每次重建）
```

- `X` `Y`：你在 `my-changes` 上的原始 commit
- `M`：将上游更新 merge 进 my-changes 的合并提交
- `R`：release 的合并提交

---

*文档版本：1.0 | 适用场景：长期维护的 GitHub Fork 项目*
