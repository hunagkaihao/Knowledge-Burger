# Swagger 简介

Swagger 是一套用于 API 开发、文档生成和交互测试的开源工具集。

Swagger最早由 Wordnik 的联合创始人 Tony Tam 在 2011 年创建。它通过简洁的 JSON 或 YAML 格式描述 API 结构，使得 API 的设计、实现和测试更加高效直观。Swagger 后来演变为 OpenAPI Specification，并成为业界广泛接受的标准。

Swagger 的主要组成部分包括：

- **Swagger UI**：Swagger UI 是一个可视化工具，可以将 OpenAPI 规范呈现为交互式 API 文档。它允许用户直接在浏览器中查看和测试 API。
- **Swagger Editor**：Swagger Editor 是一个基于浏览器的编辑器，用于编写 OpenAPI 规范。它提供实时预览和验证功能。
- **Swagger Codegen**：Swagger Codegen 可以根据 OpenAPI 规范生成服务器存根和客户端 SDK，支持 40 多种语言。
- **Swagger Hub**：Swagger Hub 是一个集成的 API 设计和文档平台，提供协作功能和云存储。

## 安装和启动 Swagger Editor

Swagger Editor 可以通过以下两种方式安装和启动：

1. 在线使用：Swagger 官网提供了[在线版 Swagger Editor](https://editor.swagger.io/)，只需要访问即可使用。这种方式不需要任何安装，可以直接使用。
2. 本地安装：Swagger 官网同样提供了本地版 Swagger Editor，可以从 [GitHub](https://github.com/swagger-api/swagger-editor) 上下载最新版。下载后，解压文件并运行以下命令启动：

```sql
npm install
npm start
```

启动 Swagger Editor 后，可以开始创建和编辑 Swagger 规范文件。以下是一些基本操作和使用方法：

### 1、新建 Swagger 规范文件

启动 Swagger Editor 后，会自动打开一个空的 Swagger 规范文件。可以点击左侧的“New Document”按钮创建一个新的 Swagger 规范文件。

### 2、编辑 Swagger 规范文件

在 Swagger Editor 中，可以方便地编辑 Swagger 规范文件。左侧是 Swagger 规范文件的树形结构，右侧是对应的 YAML 格式代码。可以通过双击左侧树形结构中的任意节点来编辑对应的 YAML 代码。编辑完成后，可以点击左上角的“Validate”按钮检查代码是否符合 Swagger 规范。

### 3、预览 Swagger 文档

在 Swagger Editor 中，可以方便地预览 Swagger 文档。可以点击左侧的“Preview”按钮，在右侧的浏览器窗口中查看 Swagger 文档的预览效果。可以在预览窗口中测试 Swagger API 的接口，查看接口返回的结果。

### 4、导入和导出 Swagger 规范文件

在 Swagger Editor 中，可以方便地导入和导出 Swagger 规范文件。可以点击左上角的“File”按钮，选择“Import URL”或“Import File”导入 Swagger 规范文件。也可以选择“Download As”导出 Swagger 规范文件。

### 5、其他功能

除了上述基本操作和使用方法，Swagger Editor 还提供了很多其他功能，例如：

- 自动完成和语法高亮；
- 支持 Swagger 2.0 和 OpenAPI 3.0 规范；
- 可以自定义样式和布局；
- 支持多种格式的数据输入和输出等等



### **🎯 可视化 API 文档有什么作用？**

传统的 API 文档往往是 Word 或 PDF，容易出现“文档写了但代码改了，文档却忘更”的情况。而 Swagger 解决了这些痛点：

1. **所见即所得的“活”文档**
   它能根据代码自动生成文档。只要后端代码更新，这个页面展示的内容就会自动同步，永远不会过时。
2. **零门槛的在线调试（最核心功能）**
   你不需要安装 Postman 或编写代码，直接在浏览器里就能测试接口。比如，你想知道“查询所有任务”这个接口返回什么数据，直接在页面上点一下就能看到 JSON 结果。
3. **清晰的接口契约**
   它明确规定了每个接口需要什么参数（如 `id` 是多少）、支持什么格式（JSON 还是表单），以及会返回什么状态码。这对前后端分离开发至关重要，前端看着这个页面就能开发，不需要一直问后端。

### **⌨️ 结合你的截图，具体怎么使用？**

让我们看着你提供的截图来实战演练一下。页面主要分为几个区域，操作逻辑如下：

**1. 识别接口分类（Tags）**
页面将接口分成了不同的模块。

- **AgvStatus**：看名字是跟“状态”相关的接口。
- **AgvTasks**：看名字是跟“任务”相关的接口。
  点击右侧的小箭头可以展开或折叠这些分类。

**2. 理解 HTTP 方法（颜色标签）**
每个接口左侧都有不同颜色的标签，代表不同的操作类型：

- **GET (蓝色)**：通常是**获取**数据。例如 `/api/AgvTasks` 可能是“获取任务列表”。
- **POST (绿色)**：通常是**新建**或**提交**数据。例如 `/api/AgvTasks` 可能是“创建一个新任务”。
- **PUT (橙色)**：通常是**修改/更新**数据。
- **DELETE (红色)**：通常是**删除**数据。

**3. 实际测试接口（三步走）**
假设你想测试截图中的 `GET /api/AgvTasks/{id}`（获取指定 ID 的任务）：

- **第一步：展开接口**
  点击该接口右侧的下拉箭头，会展开详细面板。
- **第二步：点击 "Try it out"**
  在展开的面板右上角，通常会有一个 "Try it out" 按钮，点击它，输入框就会变活。
- 第三步：填参并执行
  - 在 `id` 的输入框里填入一个数字（比如 `1`）。
  - 点击下方的 **Execute** 按钮。
  - **看结果**：页面下方会出现 `Response body`，里面就是服务器返回的 JSON 数据，同时还能看到状态码（如 `200` 表示成功）。