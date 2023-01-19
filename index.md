## react-nut
平时业务开发中用到 react ，好奇它内部是怎样运作的，故写此来探寻其核心。
### 前言
在开始前，我们需要搞清楚 react 的几个核心基本概念：
* JSX（React.createElement）
* render (VDOM => DOM)

老规矩，我们一起来看一段最简单的代码：
```js
const element = <h1 title="foo">Hello, world!</h1>
const container = document.getElementById('root')
ReactDOM.render(element, container)
```
这三行代码很简单，可以帮助我们很好的理解 react 工作的基本原理。在没有任何 react 基础的同学看来，第一行代码写错了，你怎么把 HTML/XML 语法用到了 js 中，有一定基础的同学肯定知道第一行代码是一段 jsx 代码，而 jsx 通过 Babel 等编译工具转化为 js；转换通常很简单：用 createElement 方法替换掉标签内的代码。下面用代码来表示下正确转换后的结果：
```js
React.createElement('h1', {
  title: 'foo',
}, 'Hello, world!')
// 实际上是下面这样一个对象（简化版）
const element = {
  type: 'h1',
  props: {
    title: 'foo',
    children: 'Hello, world!'
  }
}
```
在上面的例子中，这样的一个对象被称为“React 元素”。它们描述了你希望在屏幕上看到的内容。React 通过读取这样的对象，然后使用它们来构建 DOM 以及保持随时更新。

到这里， JSX 或者说 React.createElement 已经有一些了解，下面我们来看第三行代码，也就是 render 逻辑。继续来看简单的代码实现吧：
```js
// 1. 创建元素节点
const node = document.createElement(element.type)
// 2. 给元素节点增加属性
node['title'] = element.props.title
// 3. 创建子元素节点
const text = document.createTextNode('')
// 4. 给子元素节点增加内容（使用 nodeValue 更具有通用性）
text['nodeValue'] = element.props.children
// 5. 将子元素节点添加到元素节点中
node.appendChild(text)
// 6. 将元素节点添加到根容器中
container.appendChild(node)
```
到此为止，我们完成了最小单元的 React 元素渲染


