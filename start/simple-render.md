# 简单渲染

前面的测试案例里我们写了一个简单的 `<div>test</div>` 的测试案例，下面我们来实现它的代码。

## 环境准备

在开始开发之前我们先初始化一个开发环境。

https://github.com/CodeFalling/react-tiny/releases/tag/v0.1

不再阐述如何搭建环境，上面的代码包含了一个基础的版本（其实也实现完了这个 test case 的代码）。

## JSX
前面提到 JSX 实际上会被 babel 翻译成 js 代码，例如 `React.render(<div>test</div>, element)` 会被翻译成

```js
React.render(
  React.createElement(
    'div',
    // attributes，这个例子里是空的
    null,
    'test',
    // 如果还有子元素还会出现在这
  ),
  element,
)
```

## createElement

所以我们需要实现一个 `createElement` 函数把虚拟的 DOM 树构建起来，在此前我们设计一个简单的数据结构。

```js
// react-tiny 虚拟 DOM 节点的类型
const TREACT_ELEMENT_NODE_TYPE = {
    DOM: 'DOM',
};

// react-tiny 内部存储虚拟 DOM 树的数据结构，一个节点
class TReactElement {
    constructor({
        nodeType,
        nodeOrigin, // 节点的原数据，function 或者 string 等
        children,
    }) {
        this.nodeType = nodeType;
        this.nodeOrigin = nodeOrigin;
        this.children = children;
    }
}
```

`createElement` 目前非常简单，只需要能构建一下数据结构就行

```js
// <div>Test</div> => document.createElement('div', null, 'Test', 其实这里还会有其他的 child)
function createElement(nodeType, attrs, ...children) {
    if (typeof nodeType === 'string') {
        // JSX 为 <div>xxx</div> 这种普通 DOM 时
        // 对应的是 React.createElement('div', null, ...)
        return new TReactElement({
            nodeType: TREACT_ELEMENT_NODE_TYPE.DOM,
            nodeOrigin: nodeType,
            children: children,
        });
    }
}
```

## render

现在我们需要针对这个简单的虚拟 DOM 树做一个渲染功能，让他能够通过前面下的 test case。

因为现在只有 `REACT_ELEMENT_NODE_TYPE.DOM`，只需要把 DOM 树再用字符串拼出来就行了。

```js
class TReactElement {
    //...
    // 挂载节点，返回渲染的 HTML
    mount() {
        if (this.nodeType === TREACT_ELEMENT_NODE_TYPE.DOM) {
            // DOM 元素，拼装一下就可以
            const children = this.children || [];
            const childrenMounted = children.map(child => {
                if (typeof child === 'string') {
                    // <div>test</div> => React.createElement('div', null, 'test')
                    // 当子节点为字符串时，他就真的是个字符串
                    return child;
                } else {
                    return child.mount();
                }
            }).join('');
            return `<${this.nodeOrigin}>${childrenMounted}</${this.nodeOrigin}>`;
        }
    }
}

function render(tReactElement, domContainer) {
    domContainer.innerHTML = tReactElement.mount();
}
```

现在我们通过了第一条测试。

## 代码
本章代码见：https://github.com/CodeFalling/react-tiny/releases/tag/v0.1