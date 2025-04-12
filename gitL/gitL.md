# Git



## Bash

**Bash** 是 **Bourne Again Shell** 的缩写，是一种流行的 Shell 程序，兼容 Unix/Linux 系统。



## git命令

### git branch

#### 查看分支

`git branch`：显示本地分支列表

`git branch -a`：显示所有分支，包括本地分支和远程分支（远程分支以 `remotes/` 开头）

`git branch -r`：显示所有远程分支。

#### 创建分支

`git branch <branch-name>`：创建一个新的分支，但不会切换到该分支。

`git branch <branch-name> <start-point>`：从指定的起点（`start-point`）创建一个分支。

#### 删除分支

`git branch -d <branch-name>`：删除指定的分支（只能删除已被合并的分支，否则会报错）。

`git branch -D <branch-name>`：强制删除分支（即使该分支未被合并）。

#### 重命名分支

`git branch -m <new-branch-name>`：重命名当前分支。

```bash
git branch -m new-branch-name
```

`git branch -m <old-branch-name> <new-branch-name>`：重命名指定的分支。

```bash
git branch -m old-branch new-branch
```

#### 跟踪远程分支

`git branch --track <branch-name> <remote/branch>`：创建一个本地分支，并跟踪指定的远程分支。

`git branch -u <remote/branch-name>`： **将一个已经存在的本地分支** 设置为跟踪指定的远程分支。

<span style="color:red;font-weight:bolder">-u 是`--set-upstream-to=`的简写</span>





### git commit

#### 提交

```sql
git commit -m "Initial commit"
```

使用 `-m` 选项，你可以在命令行中立即附加一条提交消息，避免进入 Git 的默认编辑器（如 Vim 或 Nano）来手动输入消息。





## 绑定远程仓库

### 远程仓库为空

```bash
git init
git add .
git commite -m "init"
git remote add origin https://gitee.com/kagy/reggie-test.git
git push -u origin "master"   -- -u 是 --set-upstream 的简写

git branch -m main -- 重命名分支
```









## 案例

将远程仓库的`.idea`文件删除，保留本地

```bash
git rm -r --cached .idea
# -r：递归地将 .idea 文件夹及其所有内容（包括子文件夹和文件）从索引中删除.
# --cached：仅从 Git 的索引中删除文件，保留本地文件。
git commit -m "Remove .idea folder from version control"
#推送远程仓库
git push origin <分支名>
```

