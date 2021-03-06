# a Vue CLI preset for Pc Web Application
>基于`@vue/cli` 4.0的项目文件生成插件，<a href="https://cli.vuejs.org/zh/guide/plugins-and-presets.html#preset" target="_blank">点我</a>查看具体介绍。
## 安装
`vue create --preset DgtTechnologyDept/vuePcPreset my-project`
## 预装依赖
### ***<a href="https://vuex.vuejs.org/zh/" target="_blank">Vuex</a>***
- `preset`已将`store`分割成模块(`module`)，每个模块拥有自己的 `state`、`mutation`、`action`，建议不同模块都独立放在`src/store/modules`目录中。
- 导出`store`的`src/store/index.js`文件已通过`require.context`API加载`src/store/modules`下的所有不同模块的`state`。
- 使用时，建议在`src/store/getters.js`中暴露不同模块的`state`。

`state`暴露用例如下：
```
const getters = {
  // 暴露base模块中的status状态
  status: state => state.base.status
}
export default getters
```
组件中访问`state`用例如下：
```
this.$store.getters.status
```

---
### ***<a href="https://element.eleme.cn/#/zh-CN" target="_blank">ElementUI</a>***
该`preset`以`ElementUI`作为主要UI组件库，已借助<a href="https://github.com/ElementUI/babel-plugin-component" target="_blank">babel-plugin-component</a>配置按需引入组件，在`src/plugins/element.js`中对所需组件做增减即可。

---
### ***<a href="http://www.axios-js.com/" target="_blank">Axios</a>***
- 在`src/utils/http.js`中已针对`axios`封装请求拦截器及响应拦截器，并针对常见的请求失败响应状态码进行了字面映射。若遇到符合的请求失败响应码，会调用`ElementUI Message`组件发起提示。最后，`http.js`分别暴露了`post`及`get`方法供项目使用。
- 项目涉及的API接口建议放在`src/api`目录下，针对不同模块将API置于不同命名的`js`文件中。以下以`src/api/home.js`接口文件为例：
```
//  home.js
import { post } from '@/utils/http'
const Host = process.env.VUE_APP_HOST + '/home'

/**
 * 查询接口
 * @param {请求参数} param
 */
export function getList(param) {
  return post({
    url: Host + '/getList',
    param
  })
}
```
具体使用：
```
// home.vue
<script>
import { getList } from '@/api/home'
export default {
  mounted () {
    getList({ param: 'example' })
      .then(res => {})
      .catch(error => {})
  }
}
</script>
```
