*2025-03-21*

# Git常用操作命令

#### 查看分支信息

```bash
// 查看本地分支
git branch
// 查看远程分支
git branch -r
// 查看所有分支
git branch -a
```

#### 切换分支
```bash
// 切换到本地分支
git checkout <branch_name>
// 切换到远程分支(从远程分支创建本地分支)
git checkout -b <branch_name> origin/<branch_name>
// 从本地创建分支
git checkout -b <branch_name>
```

#### 查看当前状态
```bash
git status
```

#### 查看提交记录
```bash
git log
```

#### 获取远程信息

```bash
// 获取远程分支信息, 并删除已经删除的远程分支
git fetch -p
// 获取远程tag信息
git fetch -t
```

#### 添加文件
```bash
git add <file_name>
// 添加所有文件
git add .
```

#### 丢弃修改
```bash
git restore <file_name>
// 丢弃所有修改
git restore .
```
#### 提交修改
```bash
git commit -m <commit_message>
```

#### 拉取
```bash
git pull
// 拉取并自动合并
git pull --rebase --autostash
```

#### 推送
```bash
git push
```