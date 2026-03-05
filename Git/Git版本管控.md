![](D:\学习笔记\Picture\GitFlow原理介绍.png)

# Git 的常用分支介绍

1. **Production分支**

   也就是我们经常使用的 Master 分支，这个分支最近发布到生产环境的代码，最近发布的
   Release， 这个分支只能从其他分支合并，不能在这个分支直接修改。

2. **Develop分支**

   这个分支是我们是我们的主开发分支，包含所有要发布到下一个 Release 的代码，这个主要合
   并与其他分支，比如 Feature 分支。

3. **Feature分支**

   这个分支主要是用来开发一个新的功能，一旦开发完成，我们合并回 Develop 分支进入下一个
   Release。

4. **Release分支**

   当你需要一个发布一个新 Release 的时候，我们基于 Develop 分支创建一个 Release 分支，完
   成 Release 后，我们合并到 Master 和 Develop 分支。

5. **Hotfix分支**

   当我们在 Production 发现新的 Bug 时候，我们需要创建一个 Hotfix, 完成 Hotfix 后，我们合
   并回 Master 和 Develop 分支，所以 Hotfix 的改动会进入下一个 Release。

# Git Flow 各分支操作原理示意

1. **Master/Develop 分支**

   所有在 Master 分支上的 Commit 应该打上 Tag，一般情况下 Master 不存在 Commit，Develop 分
   支基于 Master 分支创建。

   ![](D:\学习笔记\Picture\Master Develop分支.png)

2. **Feature 分支**

   Feature 分支做完后，必须合并回 Develop 分支, 合并完分支后一般会删点这个 Feature 分支，
   毕竟保留下来意义也不大。

   ![](D:\学习笔记\Picture\Feature 分支.png)

3. **Release 分支**

   Release 分支基于 Develop 分支创建，打完 Release 分支之后，我们可以在这个 Release 分支
   上测试，修改 Bug 等。同时，其它开发人员可以基于 Develop 分支新建 Feature (记住：一旦打了
   Release 分支之后不要从 Develop 分支上合并新的改动到 Release 分支)发布 Release 分支时，合并
   Release 到 Master 和 Develop， 同时在 Master 分支上打个 Tag 记住 Release 版本号，然后可以删
   除 Release 分支了。

   ![](D:\学习笔记\Picture\Release分支.png)

4. **Hotfix 分支**

   hotfix 分支基于 Master 分支创建，开发完后需要合并回 Master 和 Develop 分支，同时在
   Master 上打一个 tag。

   ![](D:\学习笔记\Picture\Hotfix分支.png)

# Git 版本号定义

### **基于** ***\*语义化版本 SemVer\**** **规则）**

SemVer 的核心原则是：

> **只要包含“新增向后兼容的功能”（MINOR 变更），即使同时有 PATCH 变更，也必须升级 MINOR 版本。**

### 示例（当前 `V1.0.0`)

| 变更类型               | 是否存在？ | 对版本的影响                |
| ---------------------- | ---------- | --------------------------- |
| 新增电机参数功能       | ✅ 是       | → 必须升 MINOR → `v1.1.0`   |
| 修复其他 bug           | ✅ 是       | 属于 PATCH，但被 MINOR 覆盖 |
| 小优化（如日志、格式） | ✅ 是       | 也属于 PATCH，同样被覆盖    |

### 类比理解

| 发布内容            | 正确版本 | 错误示例 | 为什么错                    |
| ------------------- | -------- | -------- | --------------------------- |
| 只修 bug            | `v1.0.1` | `v1.1.0` | 过度升级，误导用户有新功能  |
| 加功能 + 修 bug     | `v1.1.0` | `v1.0.1` | 漏掉了功能变更，违反 SemVer |
| 破坏性变更 + 新功能 | `v2.0.0` | `v1.1.0` | 没体现 API 不兼容           |