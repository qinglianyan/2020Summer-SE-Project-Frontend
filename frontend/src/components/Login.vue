<template>
  <div class="login_container" background>
    <div class="login_box">
      <!-- 头像区域 -->
      <div class="avatar_box">
        <img src="../assets/logo.jpg" alt />
      </div>
      <!-- 登录表单区域 -->
      <el-form ref="loginForm" :model="loginForm" class="login_form" label-width="0px">
        <!-- 用户名 -->
        <el-form-item label prop="username">
          <el-input v-model="loginForm.username" prefix-icon="fa fa-user" placeholder="用户名" />
        </el-form-item>
        <!-- 密码 -->
        <el-form-item label prop="password">
          <el-input
            v-model="loginForm.password"
            show-password
            prefix-icon="fa fa-lock"
            placeholder="密码"
          />
        </el-form-item>
        <!-- 按钮区域 -->
        <el-form-item class="btns" label>
          <el-button type="primary" @click="login('loginForm')">登录</el-button>
          <el-button type="primary" @click="toregister">注册</el-button>
          <el-button type="info" @click="reset">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import Qs from "qs";
export default {
  data() {
    return {
      // 登录表单的数据绑定对象
      loginForm: {
        username: "",
        password: "",
      },
    };
  },
  methods: {
    tohome() {
      this.$router.push("/home");
    },
    toregister() {
      this.$router.push("/register");
    },
    reset() {
      this.$refs.loginForm.resetFields();
    },
    // 登录
    login(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          var data = Qs.stringify({
            username: this.loginForm.username,
            password: this.loginForm.password,
          });
          // 向后端发送请求
          this.$store
            .dispatch("login", data)
            .then((res) => {
              if(res.data.success == false) {
                this.$message({
                  showClose: true,
                  message: "登录失败",
                  type: "error",
                });
                this.loginForm.username = ""
                this.loginForm.password = ""
              } else {  // 登录成功
                this.$message({
                  showClose: true,
                  message: "登录成功",
                  type: "success",
                });
                this.$router.push("/home");
              }
            })
            .catch((err) => console.log(err));
        } else {
          alert("提交表单出现错误");
        }
      });
    },
  },
};
</script>

<style lang="less" scoped>
.login_container {
  // 设置背景图片
  background-image: url("../assets/backgroud.png");
  background-repeat: no-repeat;
  background-size: 100% 100%;
  -moz-background-size: 100% 100%;
  background-color: #2b4b6b;
  height: 100%;
}

.login_box {
  width: 400px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.avatar_box {
  height: 130px;
  width: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
}

.login_form {
  position: absolute;
  bottom: 5px;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}

.btns {
  display: flex;
  // 居右对齐
  justify-content: flex-end;
}
</style>
