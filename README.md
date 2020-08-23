# vue-ele-form-data-editor | vue-ele-form 的极简数据编辑器

[![MIT Licence](https://badges.frapsoft.com/os/mit/mit.svg)](https://opensource.org/licenses/mit-license.php)
[![npm](https://img.shields.io/npm/v/vue-ele-form-data-editor.svg)](https://www.npmjs.com/package/vue-ele-form-data-editor)
[![download](https://img.shields.io/npm/dw/vue-ele-form-data-editor.svg)](https://npmcharts.com/compare/vue-ele-form-data-editor?minimal=true)

## 介绍

![image](https://s1.ax1x.com/2020/08/23/dws9Qe.gif)

## 安装

```bash
yarn add vue-ele-form-data-editor
```

或者

```bash
npm install vue-ele-form-data-editor --save
```

## 注册和使用

### 全局注册

```js
import EleForm from "vue-ele-form";
import EleFormDataEditor from "vue-ele-form-data-editor";

// 注册 data-editor 组件
Vue.component("data-editor", EleFormDataEditor);

// 注册 ele-form
Vue.use(EleForm, {
  // 专门设置全局的 data-editor 属性
  "data-editor": {
    types: ["array", "object"],
    // 其它属性，同 element input 组件
    // https://element.eleme.cn/#/zh-CN/component/input
    rows: 8
  }
});
```

### 局部注册

局部注册和使用请参考: [https://www.yuque.com/chaojie-vjiel/vbwzgu/inlpxy#I5kdQ](https://www.yuque.com/chaojie-vjiel/vbwzgu/inlpxy#I5kdQ)

### 使用

```js
formDesc: {
  xxx: {
    label: 'xxx',
    // 只需要在这里指定为 data-editor 即可
    type: 'data-editor',
    attrs: {
      types: ['function', 'promise'],
      // 其它属性，同 element input 组件
      // https://element.eleme.cn/#/zh-CN/component/input
      rows: 8
    }
  }
}
```

## 示例

```html
<template>
  <el-card
    header="ele-form-data-editor 演示"
    shadow="never"
    style="max-width: 1250px;min-height: 1000px;margin: 20px auto;"
  >
    <ele-form
      :form-data="formData"
      :form-desc="formDesc"
      :span="22"
      :request-fn="handleRequest"
      @request-success="handleSuccess"
    ></ele-form>
  </el-card>
</template>

<script>
  export default {
    data() {
      return {
        formData: {
          comic: []
        },
        formDesc: {
          comic: {
            type: "data-editor",
            label: "动漫",
            attrs: {
              types: ["array"]
            }
          }
        }
      };
    },
    methods: {
      handleRequest(data) {
        console.log(data);
        return Promise.resolve();
      },
      handleSuccess() {
        this.$message.success("提交成功");
      }
    }
  };
</script>

<style>
  body {
    background-color: #f0f2f5;
  }
</style>
```

### attrs

### 参数概述

```js
attrs: {
  // 数据类型列表
  types: {
    type: Array;
  },
  // 其它属性，同 element input 组件
  // https://element.eleme.cn/#/zh-CN/component/input
}
```

## 相关链接

- [vue-ele-form](https://github.com/dream2023/vue-ele-form)
- [element-ui](https://element.eleme.cn/#/zh-CN/component/input)
