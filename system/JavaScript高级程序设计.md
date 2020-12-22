# 第6章 集合引用类型
## Object
- 表达式上下文(let a = {name:'hao'}) === 期待返回值的上下文、语句上下文(if(a.name){a.age=20})
- 对象的属性名会自动转化为字符串，调用toString()
- 对象的值可以通过`点语法||[]语法存取`,`[]`可以使用变量或者字符串,如果属性名包含关键字保留字等(如空格)可以用`[]`
## Array
- 创建数组
```js
let a = new Array(); // []
let a = new Array(4); // (4) [empty × 4]
let a = new Array('4'); // ['4']
let a = Array(4); // (4) [empty × 4]
let a = [4]; // [4]
```
- es6新增方法
```js
// from 将类数组转化为数组 第二个参数是映射函数 第三个参数指定映射函数的this(箭头函数不适用)
const a1 = [1, 2, 3, 4];
const a2 = Array.from(a1, x => x**2);
const a3 = Array.from(a1, function(x) {return x**this.exponent}, {exponent: 2});
console.log(a2);  // [1, 4, 9, 16]
console.log(a3);  // [1, 4, 9, 16]
// of把一组参数转换为数组 替换常用的Array.prototype.slice.call(arguments)
console.log(Array.of(1, 2, 3, 4)); // [1, 2, 3, 4]
console.log(Array.of(undefined));  // [undefined]
```
- map()方法会跳过数组空位 实践中最好用显示的undefined代替数组空位
```js
const options = [1,,,,5];
// map()会跳过空位置
console.log(options.map(() => 6));  // [6, undefined, undefined, undefined, 6]
// join()视空位置为空字符串
console.log(options.join('-'));     // "1----5"
```
- 数组的length不是只读的，修改length可以删除添加元素
- 数组最多可以包含4 294 967 295个元素
- 检测数组类型`value instanceof Array`或者`Array.isArray(value)`
- 迭代器方法 - keys()返回数组索引的迭代器，values()返回数组元素的迭代器，而entries()返回索引/值对的迭代器
- fill(value,start,end)
- copyWithin(待插入位置索引,start,end)
- toLocaleString()与toString()在转换数组时大部分情况一样，如果其中一项是Date对象，转换结果不同
```js
new Date().toLocaleString() // "2020/12/21 下午3:42:05"
new Date().toString() // "Mon Dec 21 2020 15:41:57 GMT+0800 (中国标准时间)"
```
1. concat
```js
const a = [1,2];
const b = a.concat(3,[5,6])
console.log(a) // [1,2]
console.log(b) // [1,2,3,5,6]
// 特殊情况
let colors = ["red", "green", "blue"];

let newColors = ["black", "brown"];
newColors[Symbol.isConcatSpreadable] = false;

let moreNewColors = {
  [Symbol.isConcatSpreadable]: true,
  length: 2,
  0: "pink",
  1: "cyan"
};

// 强制不打平数组
let colors2 = colors.concat("yellow", newColors);

// 强制打平类数组对象
let colors3 = colors.concat(moreNewColors);

console.log(colors);   // ["red", "green", "blue"]
console.log(colors2);  // ["red", "green", "blue", "yellow", ["black", "brown"]]
console.log(colors3);  // ["red", "green", "blue", "pink", "cyan"]
```
### 搜索和位置方法
- 对比执行===严格相等
1. indexOf(item,startIndex) // return -1 or index
2. lastIndexOf(item,startIndex) // return -1 or index
3. includes(item,startIndex) // return Boolen
- 断言函数
4. find((item,index,arr)=>{ return true or false }) // return item
5. findIndex((item,index,arr)=>{ return true or false }) // return index
- 迭代方法 都不改变原数组
6. every() // return Boolen
7. some() // return Boolen
8. map() // return Array
9. filter() // return Array
10. foreach() // no return
- 归并方法
11. reduce((prevItem,curItem,curIndex,arr)=>{ return newItem },startIndex) // newItem作为下一次归并的prevItem
12. reduceRight()
## Map
- get
- set
- has
- clear
- delete
- 属性 size
- 对比Object
1. 给定固定大小的内存，Map可以比Object多存储50%的键值对
2. 插入性能Map更佳
3. 大量查找时Object更佳
4. 删除Map更加
- WeakMap键只能是Object或者继承自Object的类型
## Set
- add
- has
- clear
- delete
- 属性 size
- WeakSet