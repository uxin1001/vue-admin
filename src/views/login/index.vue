<template>
  <div id="login">
    <div class="login-wrap">
      <ul class="menu-tab">
        <li
          v-for="item in menuTab"
          :key="item.id"
          :class="{ current: item.current }"
          @click="toggleMenn(item)"
        >{{ item.text }}
        </li>
      </ul>
      <!-- 表单 start -->
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        class="login-from"
        size="medium"
      >
        <el-form-item prop="username" class="item-from">
          <label>邮箱</label>
          <el-input
            type="text"
            v-model="ruleForm.username"
            autocomplete="off"
          ></el-input>
        </el-form-item>

        <el-form-item prop="password" class="item-from">
          <label>密码</label>
          <el-input
            type="password"
            v-model="ruleForm.password"
            autocomplete="off"
            minlength="6"
            maxlength="20"
          ></el-input>
        </el-form-item>

        <el-form-item prop="repeatPassword" class="item-from">
          <label>确认密码</label>
          <el-input
            type="password"
            v-model="ruleForm.repeatPassword"
            autocomplete="off"
            minlength="6"
            maxlength="20"
          ></el-input>
        </el-form-item>

        <el-form-item prop="code" class="item-from">
          <label>验证码</label>
          <el-row :gutter="11">
            <el-col :span="16">
              <div>
                <el-input v-model.number="ruleForm.code" minlength="6" maxlength="6"></el-input>
              </div>
            </el-col>
            <el-col :span="8">
              <div>
                <el-button type="success" class="block">获取验证码</el-button>
              </div>
            </el-col>
          </el-row>
        </el-form-item>
        <el-form-item>
          <el-button 
            type="danger"
            @click="submitForm('ruleForm')"
            class="login-btn block"
          >
            提交
          </el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import {
  stripscript,
  validateEmail,
  vdPassword,
  vdCode
} from "@/utils/validate";
export default {
  name: "login",

  data() {
    //验证用户名
    var validateUsername = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入用户名"));
      } else if (validateEmail(value)) {
        callback(new Error("用户名格式有误"));
      } else {
        callback(); //true
      }
    };
    //验证密码
    var validatePassword = (rule, value, callback) => {
      this.ruleForm.password = stripscript(value);
      value = this.ruleForm.password;
      if (value === "") {
        callback(new Error("请输入密码"));
      } else if (vdPassword(value)) {
        callback(new Error("密码为6-20的数字加字母"));
      } else {
        callback();
      }
    };
    //验证确认密码
    var validateRepeatPassword = (rule, value, callback) => {
      this.ruleForm.repeatPassword = stripscript(value);
      value = this.ruleForm.repeatPassword;
      if (value === "") {
        callback(new Error("请输入确认密码"));
      } else if (value != this.ruleForm.password) {
        callback(new Error("两次密码不一致，请重新输入"));
      } else {
        callback();
      }
    };
    //验证验证码
    var validateCode = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入验证码"));
      } else if (vdCode(value)) {
        callback(new Error("验证码格式有误"));
      } else {
        callback();
      }
    };
    return {
      menuTab: [
        { text: "登录", current: true },
        { text: "注册", current: false }
      ],
      ruleForm: {
        username: "",
        password: "",
        repeatPassword: "",
        code: ""
      },
      //验证规则
      rules: {
        username: [{ validator: validateUsername, trigger: "blur" }], //trigger 失去焦点触发
        password: [{ validator: validatePassword, trigger: "blur" }],
        repeatPassword: [{ validator: validateRepeatPassword, trigger: "blur" }],
        code: [{ validator: validateCode, trigger: "blur" }]
      }
    };
  },

  created() {},
  //挂载完成后自动执行的
  mounted() {},
  //写函数的地方
  methods: {
    toggleMenn(data) {
      this.menuTab.forEach(elem => {
        console.log(data);

        elem.current = false;
      });
      //高光
      data.current = true;
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
#login {
  height: 100vh;
  background-color: #334a5f;
}
.lohin-wrap {
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
.login-from {
  margin-top: 28px;
  width: 400px;
  margin: auto;

  label {
    display: block;
    font-size: 14px;
    margin-bottom: 4px;
    color: #fff;
  }
  .item-from {
    margin-bottom: 13px;
  }
  .block {
    width: 100%;
    display: block;
  }
  .login-btn {
    margin-top: 19px;
  }
}
</style>