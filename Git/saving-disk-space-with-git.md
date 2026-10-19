*2026-05-09*

# Git 节省磁盘空间

仓库越大，`.git` 占的空间越可观。省空间的思路有四种，分别应对四种情况：分支多、目录多、文件大、历史长。

<!-- --- -->

## 1. Worktree：多个分支共用一个 .git

多条分支并行开发时，把仓库目录复制几份的做法很常见。问题在于每份副本都带完整的 `.git`——500 MB 的仓库复制三份就是 1.5 GB。

worktree 让多个工作目录共享同一份对象库：

```
project/        500 MB（主工作树 + .git）
feature-a/        5 MB（只有工作区文件）
feature-b/        5 MB（只有工作区文件）
```

```bash
# 新建分支并检出（基于当前 HEAD）
git worktree add -b feature-abc ../feature-abc

# 检出已有分支
git worktree add ../feature-xyz feature-xyz

# 基于远程分支创建本地分支
git worktree add -b feature-123 ../feature-123 origin/feature-123

# 列出所有 worktree
git worktree list

# 删除 worktree
git worktree remove ../feature-abc

# 清理已失效的 worktree 记录
git worktree prune

# 移动/重命名
git worktree move ../feature-abc ../feature-abc-new
```

注意：

- 同一分支同一时间只能在一个 worktree 中检出
- 各工作目录互相独立，未提交的修改不会出现在其他 worktree；提交后经共享的 refs 立即可见
- 所有 worktree 共用同一套远程配置，push/pull 的都是同一个仓库
- 主工作树不能删除

<!-- --- -->

## 2. Sparse Checkout：只检出需要的目录

只改单体仓库中的一小块时，没必要把整个仓库都检出来。

```bash
git clone --filter=blob:none --sparse git@github.com:user/large-repo.git
cd large-repo
git sparse-checkout set src/ tests/
```

- `--filter=blob:none`：blob 不随克隆下载，检出时按需取（partial clone）
- `--sparse`：初始只检出根目录
- `set` 指定要检出的目录，之后可随时调整

```bash
# 设置检出目录
git sparse-checkout set src/ tests/

# 追加目录
git sparse-checkout add docs/

# 查看当前规则
git sparse-checkout list

# 恢复完整检出
git sparse-checkout disable
```

注意：`--filter=blob:none` 才是省下载量的关键；不加它，对象还是会完整落盘，只是工作树变小。

<!-- --- -->

## 3. Git LFS：大文件不进仓库

视频、模型、数据集这类二进制文件一旦进了 Git，历史里每个版本都永久占空间，每次克隆都要全量下载。

LFS 把大文件放到仓库外的存储服务上，仓库里只留一个几 KB 的指针文件：

```bash
# 每台机器初始化一次
git lfs install

# 按模式追踪
git lfs track "*.mp4"
git lfs track "*.zip"
git lfs track "data/*.csv"

# 也可以追踪单个文件
git lfs track path/to/large_file.bin

# 查看追踪规则
git lfs track
```

之后 add/commit/push 照常，命中的大文件自动走 LFS。

克隆默认仍会下载全部 LFS 文件，空间紧张时可以只拿指针，用到再拉实体：

```bash
# 克隆时跳过 LFS 下载
GIT_LFS_SKIP_SMUDGE=1 git clone git@github.com:user/repo.git

# 需要时再拉（--include 指定路径，不加则拉全部）
git lfs pull --include "data/*"

# 推送
git lfs push
```

注意：

- 追踪规则写在 `.gitattributes`，这个文件必须提交，否则协作者那里规则不生效
- GitHub/GitLab 的 LFS 有独立的存储配额和带宽限制

<!-- --- -->

## 4. Shallow Clone：不下载历史

历史几千个提交的老仓库，空间大多耗在历史上。只要最新代码时：

```bash
# 只要最近 1 个提交
git clone --depth 1 git@github.com:user/repo.git

# 只要最近 10 个提交，并限定分支
git clone --depth 10 --branch main git@github.com:user/repo.git

# 需要完整历史时补齐
git fetch --unshallow
```

代价是 log/blame 只能看到截断处为止，也检不出历史里的文件。适合 CI 拉代码构建，不适合要查历史的本地开发。

<!-- --- -->

## 对比

| 技术 | 省掉的空间 | 适用场景 | 主要限制 |
|-----------|---------------|----------|-------------|
| **Worktree** | 重复仓库的 `.git` 对象 | 多分支并行开发 | 同一分支不能同时检出 |
| **Sparse Checkout** | 不需要的目录 | 只改单仓库的一部分 | 省下载量须配合 `--filter` |
| **Git LFS** | 大型二进制文件 | 含媒体/数据集的项目 | 依赖服务端，配额单独算 |
| **Shallow Clone** | 提交历史 | CI/CD、快速拉代码 | 看不到历史 |

<!-- --- -->

## 组合使用

几种方式互不冲突，可以叠加：

```bash
# 单体仓库：只拿最新提交 + 只检出我的子项目
git clone --depth 1 --filter=blob:none --sparse git@github.com:user/monorepo.git
cd monorepo
git sparse-checkout set my-project/
```

```bash
# 在此基础上还要多分支并行
git worktree add -b feature-new ../feature-new
```

<!-- --- -->

## 如何选择

- 分支来回切换 → Worktree
- 单仓库只改一块 → Sparse Checkout
- 二进制大文件 → LFS
- 只要最新代码 → Shallow Clone

磁盘紧张就按需组合。
