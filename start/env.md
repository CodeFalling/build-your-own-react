# 环境准备

在开始开发之前我们先初始化一个开发环境。

## 初始化

初始化一个包

```
npm init
```

## webpack

由于 webpack4 目前已经能够提供一个足够舒适简单的约定，不需要太多配置，我们直接使用 webpack 即可。

```js
npm install webpack-cli
```

默认情况下，webpack 使用 `src/index.js` 作为入口文件，默认把 `dist` 作为输出目录。

新建 `index.js`

```js
alert('hello world')
```

## npm scripts

为了避免记忆命令，可以把 script 写到 `package.json` 里

```json
{
  "scripts": {
    "dev": "webpack -w",
    "build": "webpack"
  }
}
```

然后我们就可以通过 `npm run dev` 来启动构建监听。

## HTML

构建出 JS bundle 后我们需要用一个 HTML 来引入，直接新建一个 `index.html` 即可

```html
<html>
  <head></head>
  <body>
    <div id="root"></div>
    <script src="dist/main.js"></script>
  </body>
</html>
```

现在我们的目录结构如下

```js
├── dist
│   └── main.js
├── index.html
├── package.json
└── src
    └── index.js
```

打开 `index.html` 就能看到我们的页面。

