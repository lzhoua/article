### wepy 子组件模拟实现onShow

wepy 的组件 component 的生命周期是没有 `onShow` 这个钩子的，只有一个 `onLoad` 钩子，onload 在组件被 import 到父组件的时候就会执行，很多时候，我们并不想要这样，所以，我们需要手动实现一个 onShow 钩子；

其实方法很简单，我们只需要利用 wx:if 和 watch 钩子就能实现：

- 父组件
```html
<template>
  <view>
    <block wx:if="{{childShow}}">
      <children :onshow.sync="childShow"></children>
    </block>

    <button @tap="clickChage">点击切换</button>
  </view>
</template>
<script>
  import wepy form 'wepy'
  import children from 'children'

  exports default class Father extends wepy.page {
    data = {
      childShow: false
    }
    methods = {
      clickChage () {
        this.childShow = !this.childShow
      }
    }
  }
</script>
```
- 子组件
```html
<template>
    <view>
      我是子组件
    </view>
</template>
<script>
    import wepy form 'wepy'
    exports default class children extends wepy.component{
        props = {
          onshow: {
            type: Boolean,
            default: false
          }
        }
    
        onLoad () {}
    
        onShow () {
          console.log('模拟的onShow钩子')
        }
    
        methods = {}
    
        watch = {
          onshow (val) {
            if (val) this.onShow()
          }
        }
      }
</script>
```

