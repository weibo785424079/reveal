*fiber*

---

fiber是一个执行单元，也是一种数据结构

---

fiber，把渲染/更新过程拆分为一个个小块的任务，通过合理的调度机制来调控时间，指定任务执行的时机，从而降低页面卡顿的概率，提升页面交互体验。通过Fiber架构，让reconcilation过程变得可被中断。适时地让出CPU执行权，可以让浏览器及时地响应用户的交互

![render](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/78a602cbc87342628ace49abb5d20c39~tplv-k3u1fbpfcp-watermark.awebp)

---

```jsx
type Fber = {
    
    type: any, // 对于类组件，它指向构造函数；对于DOM元素，它指定HTML tag
    key: null | string, // 唯一标识符
    stateNode: any, // 保存对组件的类实例，DOM节点或与fiber节点关联的其他React元素类型的引用
    child: Fiber | null, // 大儿子
    sibling: Fiber | null, // 下一个兄弟
    return: Fiber | null, // 父节点
    tag: WorkTag, // 定义fiber操作的类型, 详见https://github.com/facebook/react/blob/master/packages/react-reconciler/src/ReactWorkTags.js
    nextEffect: Fiber | null, // 指向下一个节点的指针
    updateQueue: mixed, // 用于状态更新，回调函数，DOM更新的队列
    memoizedState: any, // 用于创建输出的fiber状态
    pendingProps: any, // 已从React元素中的新数据更新，并且需要应用于子组件或DOM元素的props
    memoizedProps: any, // 在前一次渲染期间用于创建输出的props
    // ……     
}
```