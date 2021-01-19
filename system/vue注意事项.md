# 开发过程中可能遇到/需要注意的问题

## devServer.proxy [看这里](https://webpack.docschina.org/configuration/dev-server/#devserverproxy)

## 接口统一管理

我们把接口统一管理目的是增强项目的可维护性、可读性，以星云模板工程为例<br/>

- src/api/module/goods.js

```js
// 导入封装好的axios
import Axios from '@/lib/request';
// 定义与goods相关的请求
const GOODS = {
  getGoods: params =>
    Axios.post(`//${process.env.VUE_APP_BASE_API}/getGoods`, params),
};
// 导出整个GOODS对象
export default GOODS;
```

- src/api/index.js

```js
// 导入划分好的模块
import goods from './module/goods';
// 使用结构语法统一注册API
const API = { ...goods };
// 默认导出API
export default API;
```

- 页面调用

```js
// 导入API对象
import API from '@/api';
// 使用trycatch配合async/await
async function getGoods() {
  const res = {},
    params = {
      page: 1,
      pageSize: 10,
    };
  try {
    res = await API.getGoods(params);
  } catch (error) {
    console.log(error);
  }
  return res;
}
```

## Vue Router

没有特殊情况的时候使用 history 模式，并且建议在 router 中导入组件的时候都写上 by`component: () => import(/* webpackChunkName: "NoticeDetail" */ '@/views/notice/Detail.vue')`，便于 webpack 生成有意义的文件名，符合语义化的规范

## 书写注意

1. v-for 一定要配合 :key 使用
2. v-for 不允许和 v-if 作用于同一标签 （考虑使用计算属性 computed）
3. 单文件组件 style 统一加上 scoped 避免影响其他组件
4. 引用了 sass 的项目按照要求使用变量做主题颜色等的样式书写
5. 组件拆分按照单一职责的原则
6. 箭头函数`() => {}`代替普通函数，避免 this 混乱难以理解
7. const/let 代替 var








