*2026-05-09*

# 使用 Git 节省磁盘空间：4 种实用技巧

当处理大型 Git 仓库时，磁盘空间很快就会成为问题。无论是处理庞大的单体仓库、包含大型二进制文件的项目，还是需要同时在多个分支上工作，Git 都提供了多种优化存储空间的方法。

在这篇文章中，我们将介绍四种有效节省 Git 磁盘空间的方法。

<!-- --- -->

## 1. Git Worktree：多分支，单仓库

**作用：** 允许你在不同目录中同时检出多个分支，所有分支共享同一个 `.git` 仓库。

**节省空间：** 每个工作树可节省 `.git` 对象数据库的大小（通常为几十到几百 MB）。

### 工作原理

不再多次克隆整个仓库：

```
# 传统方式 - 浪费空间
project/        500 MB
project-copy/   500 MB
project-branch/ 500 MB
总计: 1.5 GB
```

使用 worktree：

```
# Worktree 方式 - 共享 .git
project/        500 MB (主 worktree + .git)
feature-a/       5 MB  (worktree)
feature-b/       5 MB  (worktree)
总计: 510 MB
```

### 常用命令

```bash
# 为新分支创建 worktree
git worktree add ../feature-abc feature-abc

# 为已有分支创建 worktree
git worktree add ../feature-xyz feature-xyz

# 从远程分支创建
git worktree add ../feature-123 -b feature-123 origin/feature-123

# 列出所有 worktree
git worktree list

# 删除 worktree
git worktree remove ../feature-abc

# 清理过期 worktree
git worktree prune
```

### 要点

- 所有 worktree 共享同一个 `.git` 仓库
- 任何一个 worktree 中的更改会立即在其他 worktree 中可见
- 每个 worktree 可以独立 push/pull
- 非常适合同时在多个分支上工作
- 不能删除主工作树
- 不能直接重命名 worktree（使用 `git worktree move` 或重新创建）

<!-- --- -->

## 2. Sparse Checkout：只检出你需要的

**作用：** 只检出你需要的特定目录或文件，忽略仓库中的其他内容。

**节省空间：** 当你只需要大型仓库的一个子集时，可以节省大量空间。

### 工作原理

```bash
# 传统克隆 - 下载所有内容
git clone git@github.com:user/large-repo.git
# 下载: src/, tests/, docs/, assets/, scripts/ 等

# Sparse checkout - 只下载需要的
git clone --filter=blob:none --sparse git@github.com:user/large-repo.git
cd large-repo
git sparse-checkout set src/ tests/
# 只下载: src/ 和 tests/
```

### 常用命令

```bash
# 启用 sparse checkout 克隆
git clone --filter=blob:none --sparse <repo-url>

# 设置要包含的目录
git sparse-checkout set src/ tests/

# 添加更多目录
git sparse-checkout add docs/

# 查看当前 sparse checkout 规则
git sparse-checkout list

# 禁用 sparse checkout（下载所有内容）
git sparse-checkout disable
```

### 要点

- 适合只需要处理某个子项目的单体仓库
- 减少克隆时间和磁盘占用
- 可以动态添加/移除目录
- `.git` 目录仍包含所有对象，但工作树更小

<!-- --- -->

## 3. Git LFS：轻松管理大文件

**作用：** 将大型二进制文件（视频、模型、数据集）存储在独立服务器上，而不是 Git 仓库中。

**节省空间：** 对于包含大型二进制文件的项目，节省空间非常显著。仓库中只存储小型指针文件。

### 工作原理

```bash
# 不使用 LFS - 大文件会撑大仓库
git add big_dataset.zip  # 2GB 进入 Git
git commit
git push
# 每次克隆都要下载 2GB

# 使用 LFS - 大文件单独存储
git lfs install
git lfs track "*.zip"
git lfs track "*.mp4"
git add big_dataset.zip  # 只有小型指针（KB）进入 Git
git commit
git push
# 克隆只下载指针
git lfs pull  # 需要时下载实际文件
```

### 常用命令

```bash
# 安装并初始化 LFS
git lfs install

# 追踪特定文件类型
git lfs track "*.mp4"
git lfs track "*.zip"
git lfs track "data/*.csv"

# 追踪特定文件
git lfs track path/to/large_file.bin

# 查看追踪模式
git lfs track

# 拉取实际 LFS 文件
git lfs pull

# 推送 LFS 文件
git lfs push
```

### 要点

- 需要 LFS 服务器支持（GitHub、GitLab、Bitbucket 都支持）
- 指针文件替代仓库中的实际文件
- 文件通过 `git lfs pull` 按需下载
- 可以显著减少媒体资源丰富项目的仓库大小
- 存储配额可能与普通 Git 存储分开

<!-- --- -->

## 4. Shallow Clone：跳过历史记录

**作用：** 只下载最近的提交，跳过完整的提交历史。

**节省空间：** 对于有长历史记录的仓库（数千次提交）节省空间显著。

### 工作原理

```bash
# 完整克隆 - 下载整个历史
git clone git@github.com:user/repo.git
# 下载: 5,000 次提交，完整历史

# Shallow clone - 只下载最新提交
git clone --depth 1 git@github.com:user/repo.git
# 下载: 1 次提交，无历史
```

### 常用命令

```bash
# 只克隆最新提交
git clone --depth 1 <repo-url>

# 克隆最近 N 次提交
git clone --depth 10 <repo-url>

# 将浅克隆转换为完整克隆（获取剩余历史）
git fetch --unshallow

# 指定深度和分支克隆
git clone --depth 1 --branch main <repo-url>
```

### 要点

- 获取最新代码的最快方式
- 无法访问完整的提交历史
- 无法从旧提交创建分支
- 适合 CI/CD 和快速测试
- 可以稍后通过 `--unshallow` 转换为完整克隆

<!-- --- -->

## 对比总结

| 技术 | 节省内容 | 适用场景 | 限制 |
|-----------|---------------|----------|-------------|
| **Worktree** | 每个额外分支的 `.git` 对象 | 多分支开发 | 需要外部目录 |
| **Sparse Checkout** | 不需要的目录/文件 | 单体仓库，部分工作 | 完整 `.git` 仍存在 |
| **Git LFS** | 大型二进制文件 | 媒体资源丰富的项目 | 需要 LFS 服务器设置 |
| **Shallow Clone** | 提交历史 | 快速访问，CI/CD | 无完整历史访问 |

<!-- --- -->

## 组合使用

可以组合使用这些方法以获得最大空间效率：

```bash
# 示例：Sparse + Shallow 用于单体仓库
git clone --depth 1 --filter=blob:none --sparse git@github.com:user/monorepo.git
cd monorepo
git sparse-checkout set my-project/
```

```bash
# 示例：Worktree 用于分支切换
git worktree add ../feature-new feature-new
# 现在可以在 feature-new 上工作，无需复制 .git
```

<!-- --- -->

## 如何选择

- **需要在多个分支上工作？** → 使用 Worktree
- **大型单体仓库，只需要一部分？** → 使用 Sparse Checkout
- **项目有大型媒体文件？** → 使用 Git LFS
- **只需要最新代码？** → 使用 Shallow Clone
- **磁盘空间整体有限？** → 组合使用多种技术

<!-- --- -->

## 结语

Git 的灵活性让你能够根据具体需求和限制来定制工作流程。通过理解这些节省空间的技巧，即使处理庞大的仓库，也能在不牺牲生产力的情况下高效工作。

选择适合你用例的方法，或者组合使用以获得最大效率。你的磁盘（以及你的耐心）会感谢你！

<!-- --- -->

*快乐编码！*