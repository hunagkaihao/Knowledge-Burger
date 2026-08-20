***PR***（Pull Request)：拉取请求

**目的**：开发者可以将自己完成的代码修改提交给代码库的维护者，申请将这些更改合并到目标分支中

# 操作流程

1. **克隆项目**

2. **创建分支**
3. **修改代码并提交**
4. **同步远程仓库**
5. **推送代码**

6. **创建 Pull Request**:

- 推送完 Release 分支后，打开 GitLab 项目界面

- 选择 Release 分支，点击右上角 Compare 按钮

  ![](D:\Project\Knowledge-Burger\Picture\git\Git Compare.png)

- 点击 Merge requests 按钮进行合并请求，填写 PR 标题和描述，并选择目标分支

- 点击 Create pull request 提交

7. **审查和修改**：

- 如果维护者提出修改意见： 返回 PyCharm 修改代码。 重复提交和推送操作，GitHub 会自动更新 PR。

8. **合并或关闭PR**:

- 当 PR 获得批准后，维护者会合并代码到目标分支。
- 如果需要关闭 PR，可在 GitHub 上选择 **Close pull request**。

---

# VCS（Version Control System 版本控制系统)

**“git 中将未进行版本管控的文件通过 VCS 添加到更改的文件中”**
这句话中的 **VCS 就是指 Git 本身**（或其他版本控制系统，如 SVN、Mercurial 等）。

**详细解释**

- **VCS（Version Control System）** 是一类工具的统称，用于：
  - 跟踪文件变更；
  - 管理代码历史；
  - 支持多人协作；
  - 回滚到任意历史版本。
- 常见的 VCS 包括：
  - **Git**（分布式，最流行）
  - **Subversion (SVN)**（集中式）
  - **Mercurial**
  - **Perforce**

# 重命名仓库

**GitHub**：

1. 进入仓库页面后，点击顶部导航栏中的 **Settings**（设置）标签页。
2. 在左侧边栏中向下滚动，找到 **Repository name**（仓库名称）输入框。
3. 将当前名称 “daer” 修改为你想要的新名称（例如 “Knowledge-Burger”）。
4. 点击下方的 **Rename**（重命名）按钮确认更改。

**更新本地远程**

```git
# 查看当前远程地址
git remote -v

# 更新为新的 HTTPS 地址
git remote set-url origin https://github.com/hunagkaihao/新仓库名.git

# 或者更新为 SSH 地址（推荐）
git remote set-url origin git@github.com:hunagkaihao/新仓库名.git
```

