<template>
  <div flex2 class="about">
    <div flex1 id="header">
      <component :is="header" />
    </div>
    <div flex1 id="banner">我是banner</div>
    <div flex1 id="Components1" class="Components">组件1</div>
    <div flex1 id="foot">我是脚部</div>
  </div>
</template>
<script>
import actions from "@/shared/actions";
import Vue from "vue";
export default {
  data() {
    return {
      token: "",
      header: "header",
    };
  },
  computed: {},
  mounted() {
    // 注册观察者函数
    // onGlobalStateChange 第二个参数为 true，表示立即执行一次观察者函数
    actions.onGlobalStateChange(async (state) => {
      console.log(`state`, state);
      this.header = await this.registerComponent("header", state.header);
      const { token } = state;
      this.token = token;
      // 未登录 - 返回主页
      // if (!token) {
      //   this.$message.error("未检测到登录信息！");
      //   return this.$router.push("/");
      // }

      // 获取用户信息
      // this.getUserInfo(token);
    }, true);
  },
  methods: {
    async registerComponent(path, templateName) {
      return import(`../components/${path}/${templateName}.vue`).then(
        (component) => {
          Vue.component(templateName, component.default);
          return templateName;
        }
      );
    },
  },
};
</script>
<style lang="less" scoped>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
*[flex1] {
  display: flex;
  align-items: center;
  justify-content: center;
}
*[flex2] {
  display: flex;
  align-items: center;
  flex-direction: column;
}
* {
  max-width: 375px;
}
.about {
  width: 100%;
  #header {
    width: 100%;
  }
  #banner {
    width: 100%;
    height: 200px;
    background-color: #99f;
  }
  .Components {
    width: 100%;
    height: 200px;
    background-color: #770;
  }
  #foot {
    width: 100%;
    height: 50px;
    background-color: #8e0;
  }
}
</style>