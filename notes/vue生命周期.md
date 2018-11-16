### vue 生命周期

首先，盗用官方的一张图

<img width="400px" src="https://cn.vuejs.org/images/lifecycle.png">


从图上可以看出，vue的生命周期钩子主要有：

1. **`beforeCreate`:**（此时 `date、method` 和 `el` 均没有初始化，可以在此加载loading）
2. **`created`:**（此时date和method初始化完成，但是DOM节点并没有挂载，判断是否有 `el` 节点，如果有则编译 `template`，如果没有则使用 `vm.$mount` 创建一个默认节点，此时可以在 `DOM` 渲染之前进行数据的初始化和 `method`的自执行等）

3. **`beforeMount`:**（编译模板，并且将此时在 `el` 上挂载一个虚拟的 `DOM` 节点）
4. **`mounted`:**（编译模板，且将真实的 `DOM` 节点挂载在 `el` 上）

5. **`beforeUpdate`:**（在数据有更新时，进入此钩子函数，虚拟 `DOM` 被重新创建）
6. **`updated`:**（数据更新完成时，进入此钩子函数）

7. **`beforeDestory`:**（组件销毁前调用，此时将组件上的 `watchers` 、子组件和事件都移除掉）
8. **`destoryed`:**（组件销毁后调用）

## Vue 父子组件之间的生命周期

在Vue中，由于父元素的template模板嵌套了子元素，因此在编译模板时，会先进入到父元素的template，然后层层递归进行子元素的模板编译。

在创建时，父子组件的生命周期是：
 >父组件beforeCreated -> 父组件created -> 父组件beforeMounted -> 子组件beforeCreated -> 子组件created -> 子组件beforeMounted -> 子组件mounted -> 父组件mounted。

在销毁时，父子组件的生命周期是：
 >父组件 `beforeDestory`-> 子组件 `beforeDestoryed` -> 子组件 `destoryed` -> 父组件 `destoryed`

总之记住，父子组件的生命周期遵循：由外到内，再由内到外

