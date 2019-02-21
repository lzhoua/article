### javascript 继承
####原型链继承
```javascript
   function Parent () {
      this.name = ['longzhou']
    }
    Parent.prototype.setName = function () {
      this.name.push('change')
    }
    
    function Children () {}
    Children.prototype = new Parent()
    
    let child1 = new Children()
    let child2 = new Children()
    
    console.log('------>', child1.name) // ["longzhou"]
    console.log('------>', child2.name) // ["longzhou"]
    
    child1.setName()
    
    console.log('------>', child1.name) // ["longzhou", "change"]
    console.log('------>', child2.name) // ["longzhou", "change"]
```

> **缺点：** 多个实例对引用类型的操作会被篡改


#### 构造函数继承
    ```javascript
    function Parent () {
      this.name = ['longzhou']
      this.setName = function () {
        this.name.push('change')
      }
    }
    function Children () {
      Parent.call(this)
    }
    
    let child1 = new Children()
    let child2 = new Children()
    
    console.log('------>', child1.name) // ["longzhou"]
    console.log('------>', child2.name) // ["longzhou"]
    
    child1.setName()
    
    console.log('------>', child1.name) // ["longzhou", "change"]
    console.log('------>', child2.name) // ["longzhou"]
    ```
    
   > 优点： 
        1.避免了引用类型的属性被所有实例共享
        2.可以在 Child 中向 Parent 传参
    缺点: 
    方法都在构造函数中定义，每次创建实例都会创建一遍方法。
    

#### 组合继承

```javascript
    function Parent () {
      this.name = ['longzhou']
      this.setName = function () {
        this.name.push('change')
      }
    }
    function Children () {
      Parent.call(this)
    }
    Children.prototype = new Parent()
    Children.prototype.constructor = Parent
    
    let child1 = new Children()
    let child2 = new Children()
    
    console.log('------>', child1.name) // ["longzhou"]
    console.log('------>', child2.name) // ["longzhou"]
    
    child1.setName()
    
    console.log('------>', child1.name) // ["longzhou", "change"]
    console.log('------>', child2.name) // ["longzhou"]

```

