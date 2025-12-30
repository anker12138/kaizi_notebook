Git 是分布式版本控制系统，以下是按**使用场景分类**的常用命令，覆盖从基础操作到高级协作的核心场景，附关键说明：

### 一、基础配置（首次使用必做）
```bash
# 配置全局用户名（关联提交记录）
git config --global user.name "Your Name"

# 配置全局邮箱（关联Git账号）
git config --global user.email "your.email@example.com"

# 查看配置信息（验证是否生效）
git config --list

# 配置默认编辑器（如VS Code）
git config --global core.editor "code --wait"
```

### 二、仓库初始化与克隆
```bash
# 本地新建仓库（初始化.git目录）
git init

# 克隆远程仓库（HTTPS方式，通用）
git clone https://github.com/用户名/仓库名.git

# 克隆远程仓库（SSH方式，免密，需配置SSH密钥）
git clone git@github.com:用户名/仓库名.git

# 克隆指定分支
git clone -b 分支名 仓库地址
```

### 三、文件状态与暂存（工作区→暂存区）
```bash
# 查看文件状态（红色=未追踪，绿色=已暂存）
git status

# 查看简洁版状态
git status -s

# 添加单个文件到暂存区
git add 文件名.txt

# 添加指定目录到暂存区
git add 目录名/

# 添加所有修改/新增文件到暂存区（推荐）
git add .

# 添加所有修改（包括删除）
git add -A

# 撤销暂存区的文件（回到工作区）
git reset HEAD 文件名.txt

# 撤销工作区的修改（恢复到最近一次提交）
git checkout -- 文件名.txt
```

### 四、提交（暂存区→本地仓库）
```bash
# 提交暂存区文件到本地仓库，带提交信息（必填）
git commit -m "feat: 新增用户登录功能"

# 提交时跳过暂存区（直接提交工作区已追踪的修改）
git commit -am "fix: 修复登录按钮样式问题"

# 修改最后一次提交（未推送到远程时使用）
git commit --amend

# 查看提交历史（详细版）
git log

# 查看简洁版提交历史（一行一条）
git log --oneline

# 查看分支合并历史（图形化）
git log --graph --oneline --all

# 查看指定文件的提交历史
git log 文件名.txt
```

### 五、分支操作（核心协作功能）
```bash
# 查看本地分支（*标记当前分支）
git branch

# 查看所有分支（本地+远程）
git branch -a

# 创建新分支
git branch 分支名

# 创建并切换到新分支
git checkout -b 分支名
# 或（Git 2.23+推荐）
git switch -c 分支名

# 切换到已有分支
git checkout 分支名
# 或
git switch 分支名

# 删除本地分支（需先切换到其他分支）
git branch -d 分支名

# 强制删除未合并的本地分支
git branch -D 分支名

# 删除远程分支
git push origin --delete 分支名

# 重命名本地分支
git branch -m 旧分支名 新分支名

# 合并指定分支到当前分支（如dev合并到main）
git merge 分支名

# 取消合并（合并冲突时）
git merge --abort
```

### 六、远程仓库操作（本地→远程）
```bash
# 查看关联的远程仓库
git remote -v

# 添加远程仓库（命名为origin）
git remote add origin 仓库地址

# 修改远程仓库地址
git remote set-url origin 新仓库地址

# 拉取远程分支最新代码（合并到本地）
git pull origin 分支名

# 推送本地分支到远程（首次推送需-u关联）
git push -u origin 分支名

# 推送已关联的分支
git push

# 拉取远程所有分支信息（不合并）
git fetch

# 查看远程分支与本地分支的关联
git branch -vv
```

### 七、撤销与回滚（谨慎使用）
```bash
# 回滚到指定提交（保留修改，回到暂存区）
git reset --soft 提交ID

# 回滚到指定提交（删除暂存区，保留工作区）
git reset --mixed 提交ID  # 默认方式

# 强制回滚到指定提交（删除所有修改，慎用）
git reset --hard 提交ID

# 恢复已删除的提交（reset后找回）
git reflog  # 查看操作日志，找到提交ID
git reset --hard 提交ID

# 撤销最近一次提交（生成新提交，保留历史）
git revert 提交ID
```

### 八、暂存工作区（临时切换分支用）
```bash
# 暂存当前工作区的修改
git stash

# 查看暂存列表
git stash list

# 恢复最近一次暂存（保留暂存记录）
git stash apply

# 恢复最近一次暂存（删除暂存记录）
git stash pop

# 删除指定暂存
git stash drop stash@{0}

# 删除所有暂存
git stash clear
```

### 九、标签（版本发布用）
```bash
# 创建轻量标签（仅标记提交）
git tag v1.0.0

# 创建附注标签（带备注，推荐）
git tag -a v1.0.0 -m "发布v1.0.0版本"

# 查看所有标签
git tag

# 推送标签到远程
git push origin v1.0.0

# 推送所有标签到远程
git push origin --tags

# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
```

### 十、进阶：变基（整理提交历史）
```bash
# 变基（将当前分支的提交应用到目标分支）
git rebase 目标分支

# 中止变基（冲突时）
git rebase --abort

# 解决冲突后继续变基
git add .
git rebase --continue
```

### 核心概念速记
- **工作区**：本地修改的文件目录；
- **暂存区**：git add 后的临时存储区；
- **本地仓库**：git commit 后的本地版本库；
- **远程仓库**：GitHub/GitLab 等远程服务器的仓库。

### 常用组合命令
```bash
# 拉取最新代码→创建新分支→开发→提交→推送
git pull origin main
git checkout -b feature/xxx
# 开发修改...
git add .
git commit -m "feat: 新增xxx功能"
git push -u origin feature/xxx

# 日常同步远程代码
git fetch origin
git pull origin 分支名
```