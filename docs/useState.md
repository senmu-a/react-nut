## useState

> version 18.2.0

### 前言

我们平时在使用 `useState` 时的关注点一般就是入参与返回值还有更新/setState，那我们一起来看看从初始化 `useState` 到更新一个状态的整个流程。

### 初始化

` const [count, setCount] = useState(0) `

我们从上面👆的例子开始，`useState` 内部会拿到 `dispatcher`，然后返回调用 `dispatcher.useState()` 的结果。那么疑问点来了，这个 `dispatcher` 到底怎么来的？（devtool 注入的，需要去 react-dom 中去找）

那么 `dispatcher.useState()` 调用的结果是什么呢？我相信大家肯定知道，是个数组，第一个变量时值，第二个是改变变量值的方法。对了，如果我们再往深的看看就会发现实际上就是 `mountState()` 函数的返回值。下面深入看看 `mountState()` 内部吧。

### mountState

1. 初始化 hook 对象
2. 将我们传入的值传给 `hook.memoizedState`，之后返回值的第一个值就是该值
3. 初始化队列并且赋值给 `hook.queue`
4. 定义 `dispatch` 为 `dispatchSetState` 函数
5. 返回 `[hook.memoizedState, dispatch]`

所以，现在重点就变为了 `dispatchSetState` 内部做了哪些处理来让我更新变量值并且渲染到页面中的。

### dispatchSetState

1. 判断当前是否在 `render` 阶段更新，如果是的话先执行渲染阶段更新(`enqueueRenderPhaseUpdate`)
2. `enqueueConcurrentHookUpdate`，执行 `enqueueUpdate`（加入更新队列），返回 `getRootForUpdatedFiber()` 函数的执行结果（如果是根节点则返回根节点的 `stateNode`，否则返回 `null`）
3. 执行任务调度