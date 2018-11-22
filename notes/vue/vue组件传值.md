<h3 style="color: #6691ed">vue 组件传值总结</h3>

#### 1. props 和 $emit
父子组件之间的传值，用的最多的就是 props 和 $emit
> 父 -> 子： props
> 子 -> 父： $emit

**父组件**
```javascript
<template>
  <div class='father'>
    <label>父：</label><input v-model="fatherVal"/>

    <child :childmsg="fatherVal" @postFaher="getChildVal"></child>
  </div>
</template>

<script>
import child from './child'

export default {
  components: {
    child
  },
  data () {
    return {
      fatherVal: ''
    }
  },
  methods: {
    getChildVal (val) {
      this.fatherVal = val
    }
  }
}
</script>
```


**子组件：**
```javascript
<template>
  <div class='child'>
    <label>子：</label><input v-model="childVal" @input="changeInput"/>
  </div>
</template>

<script>
export default {
  data () {
    return {
      childVal: this.childmsg
    }
  },
  props: ['childmsg'],
  methods: {
    changeInput () {
      this.$emit('postFaher', this.childVal)
    }
  },
  watch: {
    childmsg (val) {
      this.childVal = val
    }
  }
}
</script>
```
 效果
 
---

![Kapture 2018-11-22 at 13.55.00](media/Kapture%202018-11-22%20at%2013.55.00.gif)

---

#### 2. `$attrs` 和 `$listeners` 隔代组件传值 
>第一种方式处理父子组件之间的数据传输有一个问题：如果父组件A下面有子组件B，组件B下面有组件C,这时如果组件A想传递数据给组件C怎么办呢？ 

如果采用第一种方法，我们必须让组件A通过prop传递消息给组件B，组件B在通过prop传递消息给组件C；要是组件A和组件C之间有更多的组件，那采用这种方式就很复杂了。Vue 2.4开始提供了`$attrs`和`$listeners`来解决这个问题，能够让组件A之间传递消息给组件C。

**A组件**
```html
<template>
  <div>
    <label>A:</label>
    <input v-model="val"/>
    <p>这是B组件传来的数据：{{msgB}}</p>
    <p>这是C组件传来的数据：{{msgC}}</p>
    <hr/>
    <B :msgB="val" :msgC="val" @getB="getB" @getC="getC"></B>
  </div>
</template>

<script>
import B from './B'
export default {
  components: { B },
  data () {
    return {
      val: '',
      msgB: '',
      msgC: ''
    }
  },
  methods: {
    getB (val) {
      this.msgB = val
    },
    getC (val) {
      this.msgC = val
    }
  }
}
</script>
```
**B组件**
```html
<template>
  <div>
    <label>B:</label>
    <input v-model="val" @input="passB">
    <p>这是A组件传来的数据：{{msgB}}</p>
    <p>这是C组件传来的数据：{{msgC}}</p>
    <hr/>
    <C v-bind="$attrs" v-on="$listeners" :msgB="val" @getC="getC"/>
  </div>
</template>

<script>
import C from './C'
export default {
  components: { C },
  props: ['msgB'],
  data () {
    return {
      val: '',
      msgC: ''
    }
  },
  methods: {
    passB () {
      this.$emit('getB', this.val)
    },
    getC (val) {
      this.msgC = val
    }
  }
}
</script>
```

**C组件**
```html
<template>
  <div>
    <label>C:</label>
    <input v-model="val" @input="passC">
    <p>这是A组件传来的数据：{{$attrs.msgC}}</p>
    <p>这是B组件传来的数据：{{msgB}}</p>
  </div>
</template>

<script>
export default {
  props: ['msgB'],
  data () {
    return {
      val: ''
    }
  },
  methods: {
    passC () {
      this.$emit('getC', this.val)
    }
  }
}
</script>
```
---

![Kapture 2018-11-22 at 16.57.19](media/Kapture%202018-11-22%20at%2016.57.19.gif)

---

#### 中央事件总线
>上面两种方式处理的都是父子组件之间的数据传递，而如果两个组件不是父子关系呢？这种情况下可以使用中央事件总线的方式。新建一个Vue事件bus对象，然后通过`bus.$emit`触发事件，`bus.$on`监听触发的事件。

```javascript
Vue.component('brother1', {
    data() {
      return {
        mymessage: 'hello brother1'
      }
    },
    template:`
      <div>
        <label>brother1: </label><input v-model="mymessage" @input="passData(mymessage)"> 
      </div>
    `,
    methods: {
      passData(val) {
        //触发全局事件globalEvent
        bus.$emit('globalEvent', val)
      }
    }
  })
  
  <!----------------------分割线----------------------->
  
  Vue.component('brother2', {
    template:`
      <div>
        <span>brother2: </span> <span>brother1传递过来的数据：{{brothermessage}}</span>
      </div>
    `,
    data() {
      return {
        brothermessage: ''
      }
    },
    mounted() {
      //绑定全局事件globalEvent
      bus.$on('globalEvent', (val)=>{
        this.brothermessage = val;
      })
    }
  })
  
  //中央事件总线
  let bus = new Vue();

  let app = new Vue({
    el:'#app',
    template:`
      <div>
        <brother1></brother1>
        <br/>
        <brother2></brother2>
      </div>
    `
  })
```
---

![Kapture 2018-11-22 at 14.42.30](media/Kapture%202018-11-22%20at%2014.42.30.gif)

---


#### 4.provide 和 inject 
>父组件中通过provider来提供变量，然后在子组件中通过inject来注入变量。不论子组件有多深，只要调用了inject那么就可以注入provider中的数据。而不是局限于只能从当前父组件的prop属性来获取数据，只要在父组件的生命周期内，子组件都可以调用。

**父组件**
```html
<template>
  <div class='father'>
    父：<span>{{num}}</span>
    <hr/>
    <child></child>
  </div>
</template>

<script>
import child from './child'

export default {
  components: {
    child
  },
  provide () {
    return {
      add: this.chageAdd,
      subtract: this.changeSubtract
    }
  },
  data () {
    return {
      num: 0
    }
  },
  methods: {
    chageAdd () {
      this.num += 1
    },
    changeSubtract () {
      this.num -= 1
    }
  }
}
</script>
```

**子组件**
```html
<template>
  <div class='child'>
    <button @click="addNum">+</button>
    <button @click="subtractNum">-</button>
  </div>
</template>

<script>
export default {
  inject: ['add', 'subtract'],
  methods: {
    addNum () {
      this.add()
    },
    subtractNum () {
      this.subtract()
    }
  }
}
</script>
```

---

![Kapture 2018-11-22 at 15.16.56](media/Kapture%202018-11-22%20at%2015.16.56.gif)

---
