### virtual DOM
[深度理解 Virtual DOM](https://www.cnblogs.com/wubaiqing/p/6726429.html)
[解析vue2.0的diff算法](https://segmentfault.com/a/1190000008782928)

- 技术发展历史
![](media/15428663619988.jpg)

    1. 直接用js操作dom，在大量数据的情况下，监听事件和dom操作变多，难以维护。
    2. mvc、 mvp架构的出现并没有减少所需要维护的状态，也没有降低状态更新时需要对页面的更新操作。
    3. 使用模板引擎会重新渲染整个视图，在某单个或者少部分修改时候，会造成性能浪费。

- virtual DOM算法
```html
    <ul id="ul-warp">
        <li class="item1">item1</li>
        <li class="item2">item2</li>
        <li class="item3">item3</li>
    </ul>
```

    对应Virtual DOM 算法：
    ```javascript
    let element = {
        tagName: 'ul',
        id: 'ul-warp',
        className: '',
        children: {
            {tagName: 'li', id: '', className: 'item1', children: ["item1"]},
            {tagName: 'li', id: '', className: 'item2', children: ["item2"]},
            {tagName: 'li', id: '', className: 'item3', children: ["item3"]},
        }
    }
    ```
    
    DOM 我们都可以用 JavaScript 对象来表示。那反过来，就可以用JavaScript 对象表示的树结构来构建一个真正的 DOM 。当状态变更时，重新渲染这个 JavaScript 的对象结构，实现视图的变更，结构根据变更的地方重新渲染。
    
    这就是所谓的 Virtual DOM 算法：

    > 用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中当状态变更时，重新构造一棵新的对象树。然后用新的树和旧的树进行比较两个数的差异。然后把差异更新到久的树上，整个视图就更新了。Virtual DOM 本质就是在 JS 和 DOM 之间做了一个缓存。既然已经知道 DOM 慢，就在 JS 和 DOM 之间加个缓存。JS 先操作 Virtual DOM 对比排序/变更，最后再把整个变更写入真实 DOM。