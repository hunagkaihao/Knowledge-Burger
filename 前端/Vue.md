# Vite

`概念`：Vite 是一个现代化的前端构建工具，旨在通过利用现代浏览器的原生 ES 模块支持，提供快速的开发体验。

Vite 由两部分组成：

- **开发服务器：** 基于原生 ES 模块，提供超快的热更新。
- **构建命令：** 使用 Rollup 打包代码，生成适用于生产环境的优化静态资源。

### Vite 的工作原理

Vite 的工作原理可以分为开发模式和生产模式：

- **开发模式：**
  - Vite 启动一个开发服务器，利用浏览器原生支持 ES 模块的特性，直接加载源代码。
  - 当代码发生变化时，Vite 只会更新修改的模块，并通知浏览器进行热更新，保持应用状态。
- **生产模式：**
  - Vite 使用 Rollup 打包代码，生成优化后的静态资源文件。
  - 这些文件可以部署到任何静态文件服务器上。

### Vite 适用场景

Vite 适用于各种项目，尤其适合：

- **单页应用 (SPA)：** 如 Vue、React 项目。
- **静态网站：** 快速构建博客、文档等。
- **库开发：** 利用 Vite 的构建功能，高效开发和打包库。

## 创建 Vite 项目

Vite 提供了多种方式来创建新项目，最简单的方式是使用命令行工具。

打开终端或命令行工具，运行以下命令来创建一个新的 Vite 项目：

```Vue
npm create vite@latest
```

按照提示输入项目名称并选择模板。

Vite 提供了多种模板，包括：

- **vanilla:** 纯 JavaScript 项目
- **vue:** Vue.js 项目
- **react:** React 项目
- **preact:** Preact 项目
- **lit:** Lit 项目
- **svelte:** Svelte 项目

![](D:\Project\Knowledge-Burger\Picture\前端\创建Vite项目.png)

选择模板后，Vite 会自动创建项目目录并安装依赖，本章节我们选择了 Vue 框架。

如果暂时不懂的怎么选择，一路回车也行，窗口输出的信息类似如下：

![](D:\Project\Knowledge-Burger\Picture\前端\创建Vite项目2.png)

使用 Vite 创建的项目通常包含以下文件和文件夹：

![](D:\Project\Knowledge-Burger\Picture\前端\创建Vite项目3.png)

- **node_modules:** 存放项目依赖的文件夹。
- **public:** 存放静态资源的文件夹，例如图片、字体等。
- **src:** 存放项目源代码的文件夹。
  - **main.js:** 项目入口文件。
  - **App.vue:** Vue 项目根组件。
- **index.html:** 项目首页。
- **package.json:** 项目配置文件，包含项目信息、依赖和脚本命令。
- **vite.config.js:** Vite 配置文件，用于配置 Vite 的各种选项。

### 启动开发服务器

进入项目目录：

```
cd runoob-vite-test
```

安装依赖：

```
npm install
```

运行以下命令启动开发服务器：



npm run dev

执行后，出现如下信息：

```
VITE v6.2.0  ready in 684 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  Vue DevTools: Open http://localhost:5173/__devtools__/ as a separate window
  ➜  Vue DevTools: Press Option(⌥)+Shift(⇧)+D in App to toggle the Vue DevTools
  ➜  press h + enter to show help
```

Vite 会启动一个本地开发服务器，并打印出访问地址，例如 **http://localhost:5173**，端口可以在 vite.config.js 中配置修改。

打开浏览器访问该地址，即可看到你的 Vite 项目。

![](D:\Project\Knowledge-Burger\Picture\前端\创建Vite项目4.png)

### 修改代码并查看效果

打开 src/App.vue 文件，修改代码并保存文件。

![](D:\Project\Knowledge-Burger\Picture\前端\创建Vite项目5.png)

你会发现浏览器会自动刷新，并显示修改后的效果。

![](D:\Project\Knowledge-Burger\Picture\前端\创建Vite项目6.png)