---
typora-root-url: ..
---

### 环境

1.  安装 vue

   npm install -g @vue/cli

2. 卸载 vue

   npm uninstall vue-cli -g
   
3.  运行

    设置 vue.config.js 

    ```
    module.exports={
    devServer:{
    port:3333, //启动端口
    open:true  //打开浏览器
    }
    }
    ```

    npm run serve

4.  创建项目

    vue create  vue-manage-system
    
    也可通过 npm ui 可视化创建项目
    
    ### 相关知识
    
    1.路由
    
    安装  vue-router
    
    在  router 文件夹下建立  index.js 文件 配置路由
    
    ```
    import Vue from "vue";
    import VueRouter from "vue-router";
    
    Vue.use(VueRouter);
    
    export default new VueRouter({
      routes: [{ path: "/", component: () => import("@/views/login/Index") }]
    });
    
    ```
    
    2.VueX
    
    安装 vuex  
    
    在 store 文件夹下建立 index.js
    
    3.安装  axios 和  vue-axios
    
    4.在main.js 中导入  router，store，axios，vue-axios，element-ui
    
    ```
    import Vue from "vue";
    import App from "./App.vue";
    import router from "./router";
    import axios from "axios";
    import VueAxios from "vue-axios";
    import store from "./store";
    import ElementUI from "element-ui";
    import "element-ui/lib/theme-chalk/index.css";
    
    Vue.config.productionTip = false;
    Vue.use(VueAxios, axios);
    
    Vue.use(ElementUI);
    
    new Vue({
      router,
      store,
      render: h => h(App)
    }).$mount("#app");
    
    ```
    
    6.在 App.vue 启动页面按路由启动     <router-view />
    
    ```
    <template>
      <div id="app">
        <router-view />
      </div>
    </template>
    <script></script>
    <style></style>
    
    ```
    
    7.在 View 文件夹里 创建视图 使用 element-ui   <https://element.eleme.cn/>

```
<template>
  <el-form
    class="login"
    ref="form"
    :model="sizeForm"
    :rules="rules"
    label-width="80px"
    size="mini"
  >
    <h3 class="login-title">欢迎登录</h3>
    <el-form-item label="用户名" prop="name">
      <el-input placeholder="请输入用户名" v-model="sizeForm.name"></el-input>
    </el-form-item>
    <el-form-item label="密码" prop="pass">
      <el-input
        type="password"
        placeholder="请输入密码"
        v-model="sizeForm.pass"
      ></el-input>
    </el-form-item>
    <el-form-item size="large">
      <el-button type="primary" @click="onSubmit('form')">登录</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
export default {
  data() {
    return {
      sizeForm: {
        name: "",
        pass: ""
      },
      rules: {
        name: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 3, max: 14, message: "长度在 3 到 14 个字符", trigger: "blur" }
        ],
        pass: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 3, max: 14, message: "长度在 3 到 14 个字符", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    onSubmit(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          debugger;
          this.axios({
            headers: {
              "Content-Type": "application/x-www-form-urlencoded"
            },
            method: "get",
             url: "https://api.it120.cc/cbb5173ea5b78d3b3f49f58f91cf57f9/friendly-partner/list",
           data: {
              mobile: "18603026525"
            }
          }).then(function(res) {
            debugger;
            alert(res.data.data);

            alert(res.data.msg);
          });
        } else {
          this.$message.error("账号或密码错误！");
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.login {
  width: 500px;
  height: 300;
  border: 1px solid #dcdfe6;
  margin: 150px auto;
  padding: 20px 50px 20px 30px;
  border-radius: 20px;
  box-shadow: 0px 0px 20px #dcdfe6;
}
.login-title {
  text-align: center;

  margin-bottom: 40px;
}
</style>

```

8.父组件像子组件传参，子组件像父组件传参

![](/image/子父组件.png)

​    子组件传参父组件2

​    采取发射的方式 

在子组件中  this.$emit(“键”，“值”)

在父组件中，子组件的标签中  @键=“msg:$event”   其中 $event就能得到值，msg是父组件中的 vue属性