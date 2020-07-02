<template>
  <div id="login">
    <div class="login-wrap">
      <ul class="menu-tab">
        <li class=("current":isActive) v-for="item in menuTab" :key="item.id">{{ item.text }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "login",
  data() {
    return {
      menuTab: [{ text: "登录" }, { text: "注册" }],
      isActive:true
    };
  },
  created() {},
  mounted() {},
  methods: {
    //vue 数据驱动视频渲染
  }
};
</script>

<style lang="scss" scoped>
#login {
  height: 100vh;
  background-color: #334a5f;
}
.lohin-wrap {
  width: 330px;
  margin: auto;
}
.menu-tab {
  text-align: center;
  li {
    display: inline-block;
    width: 88px;
    line-height: 36px;
    border-radius: 2px;
    font-size: 14px;
    color: #fff;
    cursor: pointer;
  }
}
.current {
  background-color: rgba(0, 0, 0, 0.1);
}
</style>