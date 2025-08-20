# Git 学习笔记

## 一、Git是什么？
Git是目前世界上最先进的分布式版本控制系统。

**工作原理 / 流程：**
- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库

## 二、SVN与Git的最主要区别
- SVN是集中式版本控制系统，版本库集中在中央服务器，必须联网。
- Git是分布式版本控制系统，每个人的电脑都是完整的版本库，协作时通过推送和拉取实现。

## 三、Windows上安装Git
1. 下载 msysgit 并默认安装。
2. 安装后在开始菜单找到 "Git Bash"，打开即安装成功。
3. 设置用户名和邮箱：
   ```bash
   git config --global user.name "你的名字"
   git config --global user.email "你的邮箱"
   ```

## 四、Git基本操作
### 1. 创建版本库
- 用 `git init` 初始化目录为Git仓库。
- 目录下会多出 `.git` 隐藏文件夹。

### 2. 添加和提交文件
- `git add 文件名` 添加到暂存区。
- `git commit -m "说明"` 提交到仓库。
- `git status` 查看状态。
- `git diff 文件名` 查看修改内容。

### 3. 版本回退
- `git log` 查看历史记录。
- `git log --pretty=oneline` 简化显示。
- `git reflog` 查看所有操作记录，可还原此前终端的记录。`git log`只能查看当前终端的记录。
- `git reset --hard HEAD^` 回退到上一个版本。
- `git reset --hard 版本号` 回退到指定版本。

### 4. 工作区与暂存区区别
- 工作区：你看到的目录和文件。
- 暂存区：用 `git add` 添加的内容。
- 仓库区：用 `git commit` 提交的内容。

### 5. 撤销修改和删除文件
- 撤销工作区修改：`git checkout -- 文件名`
- 删除文件：`rm 文件名`，然后 `git commit`。
- 恢复被删文件：`git checkout -- 文件名`

## 五、远程仓库
1. 注册GitHub账号，配置SSH Key。
2. 添加远程库：
   ```bash
   git remote add origin 仓库地址
   ```
3. 推送本地内容到远程：
   ```bash
   git push -u origin master
   ```
4. 克隆远程库：
   ```bash
   git clone 仓库地址
   ```

## 六、分支管理
- 创建分支：`git branch 分支名`
- 切换分支：`git checkout 分支名`
- 创建并切换：`git checkout -b 分支名`
- 合并分支：`git merge 分支名`
- 删除分支：`git branch -d 分支名`
- 查看分支：`git branch`

### 解决冲突
- 合并冲突时，手动编辑冲突文件，保存后提交。
- 禁用快进合并：`git merge --no-ff -m "注释" 分支名`

## 七、Bug分支与现场管理

### 1. Bug分支工作流程
在开发中经常会遇到紧急bug需要修复，但当前工作还没完成无法提交。Git提供了强大的分支和储藏功能来处理这种情况。

**场景示例：**
- 正在dev分支开发新功能，工作进行到一半
- 突然接到紧急bug报告，需要立即修复
- 但当前代码还不完整，不适合提交

### 2. 储藏工作现场
```bash
# 查看当前工作状态
git status

# 储藏当前工作现场
git stash

# 查看储藏列表
git stash list

# 给储藏添加描述信息
git stash save "描述信息"
```

### 3. 创建Bug分支修复
```bash
# 切换到主分支（或需要修复的分支）
git checkout master

# 创建并切换到bug修复分支
git checkout -b issue-404

# 修复bug后提交
git add .
git commit -m "fix: 修复404错误"

# 切换回主分支
git checkout master

# 合并bug修复
git merge --no-ff -m "merge bug fix 404" issue-404

# 删除bug分支
git branch -d issue-404
```

### 4. 恢复工作现场
```bash
# 切换回开发分支
git checkout dev

# 查看储藏列表
git stash list

# 恢复工作现场（保留stash）
git stash apply

# 恢复工作现场（删除stash）
git stash pop

# 恢复指定的stash
git stash apply stash@{0}

# 删除指定的stash
git stash drop stash@{0}

# 清空所有stash
git stash clear
```

### 5. 高级Stash操作
```bash
# 储藏时包括未跟踪的文件
git stash -u

# 储藏时包括忽略的文件
git stash -a

# 查看stash的具体内容
git stash show -p stash@{0}

# 从stash创建分支
git stash branch <branchname> stash@{0}
```

## 八、多人协作

### 1. 远程仓库管理
```bash
# 查看远程仓库信息
git remote -v

# 添加远程仓库
git remote add origin <url>

# 重命名远程仓库
git remote rename origin upstream

# 删除远程仓库
git remote remove origin

# 查看远程仓库详细信息
git remote show origin
```

### 2. 分支推送策略
```bash
# 推送本地分支到远程
git push origin <branch-name>

# 首次推送并建立跟踪关系
git push -u origin <branch-name>

# 推送所有分支
git push origin --all

# 强制推送（谨慎使用）
git push origin <branch-name> --force

# 删除远程分支
git push origin --delete <branch-name>
```

### 3. 分支拉取与同步
```bash
# 获取远程仓库更新（不合并）
git fetch origin

# 拉取并合并远程分支
git pull origin <branch-name>

# 拉取远程分支并创建本地分支
git checkout -b <local-branch> origin/<remote-branch>

# 设置本地分支跟踪远程分支
git branch --set-upstream-to=origin/<remote-branch> <local-branch>
```

### 4. 协作冲突解决
当多人同时修改同一文件时会产生冲突：

```bash
# 1. 拉取最新代码时发现冲突
git pull origin master
# Auto-merging file.txt
# CONFLICT (content): Merge conflict in file.txt

# 2. 查看冲突状态
git status

# 3. 手动编辑冲突文件
# <<<<<<< HEAD
# 你的修改
# =======
# 其他人的修改
# >>>>>>> commit-id

# 4. 解决冲突后添加到暂存区
git add <conflicted-file>

# 5. 提交合并
git commit -m "resolve merge conflict"

# 6. 推送到远程
git push origin master
```

### 5. 常见协作问题解决
```bash
# 推送被拒绝（远程有新提交）
git pull --rebase origin master
git push origin master

# 误推送到错误分支
git push origin :wrong-branch  # 删除远程错误分支
git push origin correct-branch # 推送到正确分支

# 撤销已推送的提交
git revert <commit-id>
git push origin master
```

## 九、.gitignore 文件与目录忽略

### 1. 创建 .gitignore 文件
要忽略特定的子文件夹或子目录，首先需要在项目根目录下创建一个名为 `.gitignore` 的文件。该文件用于存储要忽略的文件和文件夹的模式。只需在文件中列出要忽略的路径模式，Git 会自动忽略这些内容。

**示例 .gitignore 文件内容：**
```gitignore
# 忽略 build 文件夹
build/
# 忽略 dist 文件夹
dist/
# 忽略 node_modules 文件夹
node_modules/
```
在上述示例中，`/` 用于指定要忽略的子文件夹路径。可以根据需要添加更多规则。

### 2. 使用通配符
除了指定具体的文件夹路径，还可以使用通配符来忽略一类文件夹或文件：
- `*`：匹配任意数量的字符（不包括路径分隔符）。
- `?`：匹配单个字符（不包括路径分隔符）。
- `**`：匹配任意数量的子文件夹。

**示例：**
```gitignore
# 忽略所有名为 temp 的子文件夹
**/temp/
# 忽略所有以 _cache 结尾的子文件夹
**/*_cache/
# 忽略所有包含 _temp 的子文件夹
**/*_temp*/
```

### 3. 特殊规则
有时希望忽略文件夹中的所有内容，但保留文件夹本身，可以用 `!` 取消忽略：
```gitignore
# 忽略 build 文件夹下所有内容，但保留 build 文件夹本身
build/*
!build/
```

### 4. 忽略特定文件类型
可以用通配符忽略特定类型的文件：
```gitignore
# 忽略所有 .log 文件
*.log
```

### 5. 忽略文件夹下的特定文件
```gitignore
# 忽略 build 文件夹下所有 .js 文件
build/*.js
# 忽略 build 文件夹中的 main.js 文件
build/main.js
```

### 6. 组合规则
可以组合规则忽略文件夹和特定类型的文件：
```gitignore
# 忽略 dist 文件夹和所有 .txt 文件
dist/
*.txt
```

### 7. 模式匹配规则说明
在 .gitignore 文件中可以使用如下特殊字符：
- `*`：匹配零个或多个字符。
- `?`：匹配任意一个字符。
- `[abc]`：匹配 a、b 或 c。
- `[0-9]`：匹配 0 到 9。
- `**/`：匹配任意数量的子文件夹。

### 8. 实际操作步骤
1. **创建或编辑 .gitignore 文件**
   在项目根目录下新建或编辑 `.gitignore` 文件。例如：
   ```bash
   touch .gitignore
   ```
2. **添加忽略目录**
   在 `.gitignore` 文件中添加要忽略的目录路径，例如：
   ```gitignore
   logs/
   ```
   这将忽略 logs 目录及其所有内容。
3. **移除已被跟踪的目录（可选）**
   如果目录之前已被 Git 跟踪，仅添加忽略规则还不够，需要从索引中移除：
   ```bash
   git rm -r --cached logs/
   git commit -m "Stop tracking logs directory"
   ```
4. **推送更改到远程仓库（如有）**
   ```bash
   git push
   ```

**总结：**
通过在 `.gitignore` 文件中添加目录或文件路径，并确保未被 Git 跟踪，可以有效忽略不需要纳入版本控制的内容，如日志、临时文件、编译输出等。

---

## 十、常用命令速查
### 新建代码库
```bash
git init
git clone [url]
```
### 配置
```bash
git config --list
git config -e [--global]
git config [--global] user.name "[name]"
git config [--global] user.email "[email address]"
```
### 增加/删除文件
```bash
git add [file]
git rm [file]
git mv [file-original] [file-renamed]
```
### 代码提交
```bash
git commit -m [message]
git commit -a
git commit --amend -m [message]
```
### 分支
```bash
git branch
git checkout -b [branch]
git merge [branch]
git branch -d [branch]
```
### 标签
```bash
git tag
git tag [tag]
git tag -d [tag]
git show [tag]
```
### 查看信息
```bash
git status
git log
git diff
git show [commit]
git reflog
```
### 远程同步
```bash
git fetch [remote]
git pull [remote] [branch]
git push [remote] [branch]
```
### 撤销
```bash
git checkout [file]
git reset --hard [commit]
git revert [commit]
git stash
git stash pop
```

---

> **参考资料：**
> - [收藏了！Git 核心操作图解](https://mp.weixin.qq.com/s?__biz=Mzg2OTg3MTU2OQ==&mid=2247506565&idx=1&sn=3a332a80999b45008d977fb6a10a2275&source=41#wechat_redirect)
> - [龙恩的博客](http://www.cnblogs.com/tugenhua0707/p/4050072.html)
> - [阮一峰的Git命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)