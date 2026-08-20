

# Vue 概念

Vue.js（读音 /vjuː/, 类似于 view） 是一套构建用户界面的渐进式框架。

Vue 只关注视图层， 采用自底向上增量开发的设计。

Vue 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件。



# 启动 Vue 项目的标准步骤

1. **打开项目终端**
   在你的代码编辑器（如 VS Code）中打开 Vue 项目文件夹，然后打开内置的终端。

2. **安装项目依赖**
   在终端中输入以下命令并回车。这一步会根据项目中的 `package.json` 文件，下载并安装项目运行所需的所有库和工具。

   ```vue
   npm install
   ```

3. **启动开发服务器**
   依赖安装完成后，输入以下命令来启动项目：

   ```Vue
   npm run dev
   ```

4. **在浏览器中访问**
   命令成功执行后，终端会显示一个本地访问地址，通常是 `http://localhost:xxxx`（xxxx 是端口号，如 8080, 5173 等）。在浏览器中打开这个地址，就能看到你的 Vue 前端界面了。

5. **开发服务器**

   ```vue
   Ctrl + C
   ```

   



# CDN（Content Delivery Network,内容分发网络）

CDN 是一种通过在多个地理位置部署服务器来提供快速、可靠和高效的数据传输服务的网络。

在前端开发中，CDN 通常用于加速静态资源的加载速度。



# DOM（Document Object Model)

将 HTML 文档解析成一棵由节点组成的树状对象结构。



# HTML、CSS 和 JavaScript 分别扮演了什么角色

HTML 定义了文档的结构，为页面提供了基础骨架。

CSS 负责配置页面元素的布局和外观，增强了页面的视觉吸引力。

JavaScript 处理页面的动态更新和用户交互，是实现网页动态功能的核心。

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

```vue
npm run dev
```

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

打包项目：

```vue
npm run build
```

将生成一个 dist 文件

**部署方式举例：**

- **Nginx (最常用)**：将 `dist` 文件夹里的所有文件复制到 Nginx 的 `html` 目录下，配置好 Nginx 即可。

- **IIS**：在 IIS 中新建一个网站，物理路径指向 `dist` 文件夹。

- **Apache**：将文件放入 Apache 的 `htdocs` 目录。

- **Docker**：通常会写一个 Dockerfile，基于 Nginx 镜像，将 `dist` 内容复制进去。

  

### 修改代码并查看效果

打开 src/App.vue 文件，修改代码并保存文件。

![](D:\Project\Knowledge-Burger\Picture\前端\创建Vite项目5.png)

你会发现浏览器会自动刷新，并显示修改后的效果。

![](D:\Project\Knowledge-Burger\Picture\前端\创建Vite项目6.png)

# v-bind

对于 v-bind 指令，我们可以将其 v-bind 前缀省略，直接使用冒号加属性名的方式进行绑定，例如 v-bind:id = “id” 可以缩写为如下模样：

```vue
:id="id"
```

它的核心作用是**告诉 Vue 不要把后面的值当成普通字符串，而是当成一段 JavaScript 代码（变量或表达式）去执行**。



# v-on

对于网页应用，事件监听主要分为两大类：**键盘按键事件**和**鼠标操作事件**。

对于 v-on 类的事件绑定指令，可以将前缀 v-on: 使用 @ 符替代，例如 v-on:click="myFunc" 指令可以缩写成如下模样：

```vue
@click="myFunc"
```

# v-on:submit

- **`@submit`**：是 `v-on:submit` 的缩写，意思是“监听表单的提交事件”。
- **`.prevent`**：是一个**事件修饰符**，它的作用是自动调用原生 DOM 的 `event.preventDefault()`，也就是**阻止浏览器的默认行为**。

`@submit.prevent` 的作用就是：**拦截表单的默认提交行为（防止页面刷新），然后只执行你自定义的函数逻辑。**

```vue
<template>
  <!-- 监听 submit 事件，并阻止默认刷新行为 -->
  <form @submit.prevent="handleSubmit">
    <input type="text" v-model="message" />
    <button type="submit">提交</button>
  </form>
</template>

<script setup>
import { ref } from 'vue'

const message = ref('')

const handleSubmit = () => {
  console.log('表单提交了，内容是：', message.value)
  // 页面不会刷新，你可以在这里调用 axios 发送网络请求
}
</script>
```

# axios 库

Axios 是一个基于 Promise 的 HTTP 客户端，可用于浏览器和 Node.js 环境。它最大的特点是**同构性**，即同一套代码可以同时运行在前端和后端，这极大地便利了代码的复用。

# v-if

更高的切换性能消耗

# v-show

更高的初始渲染性能消耗

# v-for

**高级用法**

| 指令      | 含义                   |
| --------- | ---------------------- |
| push()    | 向列表尾部追加一个元素 |
| pop()     | 删除列表尾部的一个元素 |
| shift()   | 删除列表头部的一个元素 |
| unshift() | 向列表头部插入一个元素 |
| splice()  | 对列表进行分割操作     |
| sort()    | 对列表进行排序操作     |
| reverse() | 对列表进行逆序操作     |

# let

**`let` 是什么**：用来声明变量的。

`let` 最重要的特性是**块级作用域**。这意味着，你在哪里用 `{}` 把代码包起来（比如 `if` 判断、`for` 循环），`let` 声明的变量就只在这个“包”里面有效，出了这个包，变量就消失了。

# 存储属性

存储定义的值

# 计算属性

根据定义的计算逻辑来实时更新其值

# ref

`ref` 是 Vue 3 组合式 API 中最核心的函数之一。它的主要作用是**创建一个响应式的数据**。

**创建**：用 `ref(初始值)` 来创建一个响应式数据。

**读取/修改**：

- 在 `<script>` 的 JavaScript 代码中，必须通过 `.value` 属性来访问或修改它的值。
- 在 `<template>` 的模板中，Vue 会自动帮你解开 `.value`，所以直接用变量名即可。

**示例代码**：

```Vue
<template>
  <div>
    <!-- 在模板中，直接使用变量名，不需要 .value -->
    <p>你点击了 {{ count }} 次</p>
    <button @click="increment">点我加 1</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 1. 创建一个响应式变量 count，初始值为 0
const count = ref(0)

// 2. 定义一个函数来修改它
function increment() {
  // 在 script 中，必须通过 .value 来修改值
  count.value = count.value + 1
}
</script>
```

# \<any>

`:any` 并不是一个独立的语法，而是 **TypeScript** 中的一个**类型注解**。它通常和 `ref` 一起出现，写作 `ref<any>(...)`。

它通常作为泛型参数传递给 `ref`，用来定义 `ref` 盒子里可以装什么类型的东西。

- `ref<number>(0)`：这个盒子里只能装数字。
- `ref<string>('hello')`：这个盒子里只能装字符串。
- `ref<any>(null)`：这个盒子里**可以装任何东西**（数字、字符串、对象、数组...）。

```Vue
<template>
  <div>
    <!-- 如果 user 是 null，就显示加载中 -->
    <p v-if="!user">加载中...</p>
    
    <!-- 当 user 有值后，显示用户信息 -->
    <div v-else>
      <h3>用户：{{ user.name }}</h3>
      <p>年龄：{{ user.age }}</p>
    </div>
    <button @click="fetchUser">获取用户信息</button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

// 1. 定义一个响应式变量 user
// 我们不知道它具体是什么对象，所以先用 <any> 告诉 TypeScript “别管我”
// 初始值设为 null，表示“暂时没有数据”
const user = ref<any>(null)

async function fetchUser() {
  // 模拟从后台获取数据
  const response = await new Promise(resolve => {
    setTimeout(() => resolve({ name: '张三', age: 18, id: 1001 }), 1000)
  })
  
  // 2. 把获取到的对象赋值给 user
  // 因为定义了 <any>，所以这里可以放任何对象，TypeScript 不会报错
  user.value = response
}
</script>
```

# computed

它最核心的特点是：**它依赖的数据一旦发生变化，它就会自动重新计算并更新结果**。而且，如果你没有去修改它所依赖的数据，无论你读取它多少次，它都只会计算一次，然后直接返回缓存的结果（性能极高）。

# watch

它的核心作用是：**监听某个数据的变化，一旦数据发生改变，就立刻去执行一段特定的逻辑（比如发网络请求、操作本地存储、或者打印日志）**。
