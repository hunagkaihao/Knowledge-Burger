***PR***（Pull Request)：拉取请求

**目的**：开发者可以将自己完成的代码修改提交给代码库的维护者，申请将这些更改合并到目标分支中

### 操作流程

1. **克隆项目**

2. **创建分支**
3. **修改代码并提交**
4. **同步远程仓库**
5. **推送代码**

6. **创建 Pull Request**:

- 推送完 Release 分支后，打开 GitLab 项目界面

- 选择 Release 分支，点击右上角 Compare 按钮

  ![](D:\学习笔记\Knowledge-Burger\Picture\Git Compare.png)

- 点击 Merge requests 按钮进行合并请求，填写 PR 标题和描述，并选择目标分支

- 点击 Create pull request 提交

7. **审查和修改**：

- 如果维护者提出修改意见： 返回 PyCharm 修改代码。 重复提交和推送操作，GitHub 会自动更新 PR。

8. **合并或关闭PR**:

- 当 PR 获得批准后，维护者会合并代码到目标分支。
- 如果需要关闭 PR，可在 GitHub 上选择 **Close pull request**。

---

