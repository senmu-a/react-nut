## render

> version 18.2.0

### 前言

这里主要会讲 `render` 方法内部做了哪些操作和我们可以在 React 源码中可以学习到的东西。当然，这其中穿插的一些知识都是比较少的，让大家有个基本认知，如果需要深入研究还请自行查询相关资料。:D

### 起源

该方法目前是由 `createRoot` 创建而来，但是在当前版本中也保留了之前的用法，值得注意的是这种用法会在开发环境中报错（只是会在控制台打印错误，并不会影响流程）。

### 内部

当前版本的 `render` 很简单，只是调用了 `updateContainer` 方法，那我们一起来看看该方法内部做了哪些事情吧。

1. `requestEventTime` 拿到一个精确到毫秒的时间戳，内部使用 `performace.now` ，[详情参考](https://developer.mozilla.org/zh-CN/docs/Web/API/Performance/now) 
2. `requestUpdateLane` 暂时不太清楚是干嘛的，猜测也是跟打点，性能监控相关
3. `markRenderScheduled` 使用 `Profiler` 会执行具体方法，暂不做解析
4. `getContextForSubtree` `parentComponent` 为 `null`，暂不做解析
5. `createUpdate` 如变量名，创建更新，其实就是创建了变量 `update` 包括 `eventTime` 和 `lane` 等信息
6. `enqueueUpdate` 更新 `root` 
7. `scheduleUpdateOnFiber` 加入任务调度

此时我们需要剖析下 `Scheduler` 看看内部到底做了什么。

