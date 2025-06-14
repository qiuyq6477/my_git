# Git 思维导图
## HEAD 指针
- 符号名称，间接指向分支（默认main），指向分支顶端提交
- 切换命令
  - `git switch --detach <commit-id>`：分离头状态，直接指向提交
  - `git switch -` 或 `git switch <branch-name>`：重新连接到分支
- 定位历史提交
  - 脱字符（^）：每个^往前一个提交，如`HEAD^^^`
  - 波浪号（~）：`~数字`往前指定数量提交，如`HEAD~3`

## 分支管理
- 本质
  - 特定提交的名称标签，可通过操作移动，非整个提交序列
- 分支操作
  - 列出：`git branch`（带*为当前分支）
  - 创建并切换：`git switch -c newbranch`
- 合并方式
  - 快进式合并：被合并分支是目标分支直接后代，无冲突
  - 常规合并：无直接继承关系，自动创建合并提交（双父提交）
- 合并操作
  - 合并分支：`git merge main`（将main合并到当前分支）
  - 查看状态：`git status`（解决冲突或`git merge --abort`中止）
- 分支删除
  - 已合并分支：`git branch -d topic1`
  - 未合并分支（强制）：`git branch -D topic1`

## 远程仓库
- 基础概念
  - 远程服务器昵称，方便克隆、推送、拉取
  - "origin"为克隆时自动设置的别名
  - 引用格式：`remotename/branchname`
- 操作命令
  - 更改URL：`git remote set-url`
  - 添加远程仓库：`git remote add upstream <URL>`
  - 拉取内容：`git fetch upstream`
  - 合并远程分支：`git merge upstream/master`
- 推送操作
  - 推送分支：`git push origin main`
  - 建立跟踪关系：`git push --set-upstream origin <branchname>` 或 `git push -u origin <branchname>`
- 远程分支管理
  - 获取已删除分支：`git fetch --prune`
  - 删除远程跟踪分支：`git branch -dr origin/topic99`
  - 删除远程分支：`git push origin --delete topic99`

## 文件状态与操作
- 四种状态：未跟踪、未修改、已修改、已暂存
- 状态转换
  - 未跟踪→已暂存：`git add foo.txt`
  - 已修改→已暂存：`git add foo.txt`
  - 已修改→未修改：`git restore foo.txt`
  - 已暂存→未修改：`git commit`
  - 已暂存→已修改：`git restore --staged foo.txt`
- 特殊操作
  - 取消版本管理：`git rm --cached foo.txt`
  - 回退暂存区：`git restore --staged`
  - 回退工作区：`git restore --worktree`

## 差异比较
- 工作树与暂存区比较：`git diff`
- 暂存区与HEAD比较：`git diff --staged`

## 暂存（stash）操作
- 暂存工作树：`git stash`
- 弹出暂存：`git stash pop`（可指定`stash@{1}`或`--index 1`）
- 查看暂存列表：`git stash list`
- 删除暂存：`git stash drop`（可指定`stash@{1}`）
- 冲突处理：解决冲突后`git add`，冲突时不会自动弹出

## 回退与重置
- `git revert`
  - 回退提交：`git revert 9fef4`（可指定多个）
  - 选项：`-n`（不提交）
  - 冲突处理：`git revert --continue`/`--abort`/`--skip`
- `git reset`
  - `--soft`：重置分支指向，暂存区和工作树包含旧提交更改
  - `--mixed`：重置分支指向，暂存区为提交状态，工作树显示修改
  - `--hard`：重置所有内容到指定提交，丢失后续更改
  - 特定文件：`git reset foo.txt`（等同于`git restore --staged foo.txt`）

## 重写日志（reflog）
- 记录操作及对应提交ID，保留90天
- 孤立提交超过时间会被垃圾回收

## 补丁模式（-p开关）
- 支持命令：add、reset、stash、restore、commit等
- `git reset -p`：有选择地更改暂存区中的代码块
- `git add -p`：有选择地从工作树添加代码块到暂存区

## cherry-pick与blame
- cherry-pick
  - 应用指定提交：`git cherry-pick checkpoint`
  - 冲突处理：解决后`git cherry-pick --continue`
- blame
  - 查看文件每行修改人：`git blame --date=short --color-lines foo.py`

## 配置管理（git config）
- 查看配置：`git config list`（`--global`查看全局）
- 设置配置：`git config set --global user.name "Your Name"`
- 获取配置：`git config get user.name`
- 删除配置：`git config unset user.name`
- 编辑配置文件：`git config edit [--global]`

## 修正提交
- 修改上一次提交信息
  - 带暂存内容：`git commit --amend`
  - 自定义信息：`git commit --amend -m "新提交信息"`
  - 不修改信息：`git commit --amend --no-edit`
