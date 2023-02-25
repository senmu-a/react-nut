## FiberNode

> version 18.2.0

### 前言

学会 `FiberNode` 有利于我们更好的认识 React Fiber 是什么

### FiberNode 是什么

它是一个构造函数，内部有一些变量，由 `createFiber()` 函数返回 `FiberNode` 实例。也就是说任何需要创建一个 `FiberNode` 的地方都要调用到此方法。

### 一个 FiberNode 包括哪些部分？

1. 实例（Instance）

```ts
  this.tag = tag;
  this.key = key;
  this.elementType = null;
  this.type = null;
  this.stateNode = null;  
```

2. Fiber

```ts
  this.return = null; // 当前节点
  this.child = null; // 子节点
  this.sibling = null; // 兄弟节点
  this.index = 0;

  this.ref = null;
  this.refCleanup = null;

  this.pendingProps = pendingProps;
  this.memoizedProps = null;
  this.updateQueue = null;
  this.memoizedState = null;
  this.dependencies = null;

  this.mode = mode; // 类型模型，属于哪种节点
```

3. Effects

```ts
  this.flags = NoFlags;
  this.subtreeFlags = NoFlags;
  this.deletions = null;

  this.lanes = NoLanes; // 标记事件优先级
  this.childLanes = NoLanes;

  this.alternate = null;
```