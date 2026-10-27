# TypeScript

TypeScript 是 JavaScript 的一个超集，支持 ECMAScript 6 标准。

TypeScript 由微软开发的自由和开源的编程语言，在 JavaScript 的基础上增加了静态类型检查的超集。

TypeScript 设计目标是开发大型应用，它可以编译成纯 JavaScript，编译出来的 JavaScript 可以运行在任何浏览器上。

# 语法

## let

`let` 是用于声明**块级作用域**且**可重新赋值**的变量的关键字。它是 ES6 引入的，TypeScript 作为 JavaScript 的超集，完全支持并推荐使用它来替代老旧的 `var`。

```
// 显式指定类型
let count: number = 10;
let message: string = "Hello";

// 类型推断（推荐）
let age = 25; // TypeScript 自动推断 age 为 number 类型
let isActive = true; // 自动推断为 boolean 类型
```









