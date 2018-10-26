#### 私有变量
```javascript
// es5
{
  var a = 'long'
}
console.log(a) // long
// es6
{
  const a = 'long'
  let b = 'zhou'
}
console.log(a) //  a is not defined
console.log(b) //  b is not defined
```

#### 解构赋值
```javascript
// 普通写法
let a = 1
let b = 2
let c = 3
// 解构赋值写法
let [a, b, c] = [1, 2, 3]
```

#### 函数指定默认参数
```javascript
// es5不能直接为函数的参数指定默认值，只能采用变通的方法
function say (name, age) {
  age = age || 18
  console.log(`${name}有${age}岁了`)
}
say('longzhou') // longzhou有18岁了

// es6
function say (name, age = 18) {
  console.log(`${name}有${age}岁了`)
}
say('longzhou') // longzhou有18岁了
say('longzhou', 23) // longzhou有23岁了
```

#### 函数rest参数
```javascript
function add (...val) {
  console.log(val)
  return val[0] + val[1] + val[2]
}
console.log(add(1,2,3))  // [1, 2, 3]    6
// 利用rest参数特性可以封装一个将数组添加进目标数组的函数
function pushArr (targetArr, ...arr) {
  arr.forEach((item) => {
    targetArr.push(item)
  })
  console.log(targetArr)
}
let a = [1, 2, 3]
let b = [4, 5, 6]
pushArr(a, ...b) // [1, 2, 3, 4, 5, 6]
pushArr(b, 7, 8, 9) // [4, 5, 6, 7 ,8, 9]
```

#### Set数组去重
```javascript
let arr = [1, 3, 2, 3 ,4, 6, 3]
arr = Array.from(new Set(arr)) // 使用Array.from
arr = [...new Set(arr)] // 使用结构赋值
```

#### 箭头函数
```javascript
// es5
let add = function (a, b) {
  return a + b
}
// es6
let add = (a + b) => a + b

// 箭头函数和解构赋值配合使用
let obj = {name: 'longzhou', age: 18}
let say = ({name, age}) => {console.log(`${name}-${age}`)}
say(obj) // longzhou-18
```

#### 异步操作
```javascript
//es5
function say (name, func) {
  setTimeout(() => {
    func(name)
  }, 1000)
}
function callback (str) {
  console.log(`${str} 23岁了`)
}
say('longzhou', callback) // 1s后   longzhou23岁了

// es6
function say (name) {
  return new Promise((reslove) => {
    setTimeout(() => {
      reslove(name)
    }, 1000)
  })
}
say('longzhou').then((res) => {
  console.log(`${res} 23岁了`)
})
```
