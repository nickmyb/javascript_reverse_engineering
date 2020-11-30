基础语法
=======

## 1. Object

```javascript
// key 只能为 string
let o = {name: 'nickmyb'}

// deep copy
let o_deep_copied = JSON.parse(JSON.stringify(o))

o_deep_copied === o

// Array
let number_array = [1, 2, 3]
let item_0 = number_array[0]
// 遍历: map返回新数组,forEach不返回
let new_number_array = number_array.map(d => 2 * (d + 1))
number_array.forEach(d => 2 * (d + 1))
console.log('new_number_array =', new_number_array)
// index, delete count, *args to add
number_array.splice(1, 1, 'a', 'b')
console.log(number_array)

// Map, key可以为任意值
let m = new Map([[1, 'a'], ['2', 'b']])
m.get(1)
m.get('1')
m.set(1, 'aa')
m.clear()

let s = new Set([1, 1, 2, 3, '1'])

// 函数
// 函数声明优先级高于一切
function function_declare(params) {}

// 函数变量需要在引用前定义
const function_variable = function(params) {}

// 匿名函数
(function function_anonymous(params) {})()

// 作用域
// var: 函数内
// const, let: 块级{}
// 变量提升: 函数内的变量声明提升到函数顶部,但定义前为undefined
// 全局: 不使用 const, let, var

// 原型链
TODO

// 异步编程
TODO

// 浏览器存储
// cookies, local storage, session storage, IndexedDB

// 跨域

// webpack打包
```