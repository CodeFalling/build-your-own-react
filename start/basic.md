# 基本概念

本章节介绍一些必要的基本概念。

## JSX

React 最让人眼前一亮的莫过于 JSX 了，在 Babel 等转译工具的帮助下，JSX 能够让 JavaScript 和 HTML 模板很自然的混合在一起。而事实上对于 JSX 的解析和转译等完全是由 Babel 完成的，React 本身并不需要提供什么特别的支持。

例如

```jsx
<div>
    <span>Test</span> 
    Hey {name}
</div>
```

这样的代码经过 Babel 的转译后，得到的是

```jsx
React.createElement("div",null,
    React.createElement("span",null,"Test"),
    "Hey ",
    name);
```

[可以通过](https://zhuanlan.zhihu.com/p/28257907/</i)[Babel REPL Online](http://link.zhihu.com/?target=https%3A//babeljs.io/repl/%23%3Fpresets%3Dreact) 方便地看到 JSX 翻译的结果，这对于我们的开发很有帮助

而 React 只需提供 React.createElement 这个函数即可，所以我们要做的就是在完成 React-tiny 的 createElement 函数后，令 React.createElement 等于它。

