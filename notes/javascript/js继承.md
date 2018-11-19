### javascript 继承

1. 原型链继承
```javascript
    function Person () {
      this.name = "longzhou"
      this.age = 18
      this.arr = [1,2,3]
    }
    
    Person.prototype.getName = function () {
      console.log('------>', this.name)
    }
    
    function NewPerson () {}
    
    NewPerson.prototype = new Person()
    
    let instance = new NewPerson()
    console.log(instance.age) // 18
    console.log(instance.getName()) // longzhou
```

> **缺点：** 多个实例对引用类型的操作会被篡改
```javascript
 let a = new NewPerson()
 let b = new NewPerson()
 console.log(b.arr) // [1,2,3]
 a.arr.push(4)
 console.log(b.arr) // [1,2,3,4]
```

1. 借助构造函数继承
