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

# 模块

## CommonJS 同步加载
### 加载模块：`require('./a')`表示加载模块a.js或者是a文件夹下的index.js
1.第一次加载会被缓存
### 导出：`modele.exports={}`

# 网络请求与远程资源

## xhr
### XMLHttpRequest
```js
// 0：未初始化（Uninitialized）。尚未调用open()方法。
// 1：已打开（Open）。已调用open()方法，尚未调用send()方法。
// 2：已发送（Sent）。已调用send()方法，尚未收到响应。
// 3：接收中（Receiving）。已经收到部分响应。
// 4：完成（Complete）。已经收到所有响应，可以使用了。
let xhr = new XMLHttpRequest();
// 为保证跨浏览器兼容，onreadystatechange事件处理程序应该在调用open()之前赋值
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4) {
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
      alert(xhr.responseText);
    } else {
      alert("Request was unsuccessful: " + xhr.status);
    }
  }
};
// 三个参数：1.get/post... 2.地址 3.是否异步
xhr.open("get", "example.txt", true);
// 请求体或者null
xhr.send(null);
// 收到响应之前取消异步请求
xhr.abort();
```
### Resquest Headers
- Accept：浏览器可以处理的内容类型。
- Accept-Charset：浏览器可以显示的字符集。
- Accept-Encoding：浏览器可以处理的压缩编码类型。
- Accept-Language：浏览器使用的语言。
- Cache-Control: no-cache
- Connection：浏览器与服务器的连接类型。
- Content-Length: 0
- Content-Type: application/x-www-form-urlencoded
- Cookie：页面中设置的Cookie。
- Host：发送请求的页面所在的域。
- Origin: http://10.170.181.181
- Pragma: no-cache
- Referer：发送请求的页面的URI。注意，这个字段在HTTP规范中就拼错了，所以考虑到兼容性也必须将错就错。（正确的拼写应该是Referrer。）
- User-Agent：浏览器的用户代理字符串。
- 自定义`xhr.setRequestHeader("MyHeader", "MyValue");`在open()之后send()之前调用
### Response Headers
```js
let myHeader = xhr.getResponseHeader("MyHeader");
let allHeaders  xhr.getAllResponseHeaders();
```
### get
```js
// 查询字符串中的每个名和值都必须使用encodeURIComponent()编码，所有名/值对必须以和号（&）分隔
function addURLParam(url, name, value) {
  url += (url.indexOf("?") == -1 ? "?" : "&");
  url += encodeURIComponent(name) + "=" + encodeURIComponent(value);
  return url;
}
let url = "example.php";
// 添加参数
url = addURLParam(url, "name", "Nicholas");
url = addURLParam(url, "book", "Professional JavaScript");
// 初始化请求
xhr.open("get", url, false);
```
### FormData
```js
let data = new FormData();
// let data = new FormData(document.forms[0]);可以直接传入表单
data.append("name", "Nicholas");
```
### timeout & ontimeout
### overrideMimeType
```js
// 强制让XHR把响应当成XML处理
xhr.overrideMimeType("text/xml");
```
### 进度事件
- loadstart：在接收到响应的第一个字节时触发。
- progress：在接收响应期间反复触发。lengthComputable进度信息是否可用、position和totalSize可以显示进度条
- error：在请求出错时触发。
- abort：在调用abort()终止连接时触发。
- load：在成功接收完响应时触发。
- loadend：在通信完成时，且在error、abort或load之后触发。