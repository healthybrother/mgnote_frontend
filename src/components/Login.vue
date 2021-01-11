<template>
  <div class="login_page">
    <p style="font-size:40px;">MG Note</p>
    <el-row type="flex" align="middle" justify="center">
      <el-col :span="10">
        <el-card shadow="always">
          <el-form>
            <el-form-item>
              <el-input
                v-model="userIOM"
                clearable
                placeholder="请输入邮箱或用户名"
              ></el-input>
            </el-form-item>
            <el-form-item>
              <el-input
                v-model="password"
                type="password"
                placeholder="请输入密码"
              ></el-input>
            </el-form-item>
          </el-form>
          <el-row type="flex" justify="center">
            <el-col :span="8">
              <el-button class="login_btn" @click.native="login" type="primary"
                >登录</el-button
              ></el-col
            >
            <el-col :span="5">&nbsp;</el-col>
            <el-col :span="8">
              <el-button
                class="register_btn"
                @click.native="register"
                type="primary"
                >注册</el-button
              ></el-col
            >
          </el-row>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import qs from "qs";
export default {
  name: "login",
  data() {
    return {
      userIOM: "",
      password: "",
    };
  },
  methods: {
    is_Email(str) {
      var reg = /^(\w)+(\.\w+)*@(\w)+((\.\w+)+)$/;
      return reg.test(str);
    },
    login() {
      if (this.$options.methods.is_Email(this.userIOM)) {
        this.axios({
          method: "POST",
          url: "/user/loginByMail",
          data: {
            mail: this.userIOM,
            password: this.password,
          },
        }).then((res) => {
          console.log(res);
          if(res.status == 200){
            let userId = res.data.id;
            console.log(userId);
            this.$message({
              message: "登录成功",
              type: "success",
            })
            this.$router.push({path: "/main/${userId}"})
          }else{
            this.$message({
              message: "登录失败",
              type: "error",
            })
          }
        })
        .catch((err) => {
          console.log(err);
        });
      }else{
        this.axios({
          method: "POST",
          url: "/user/loginByName",
          data: {
            userName: this.userIOM,
            password: this.password,
          }
        }).then((res) => {
          console.log(res);
          if(res.status == 200){
            let userId = res.data.id;
            console.log(userId);
            this.$message({
              message: "登录成功",
              type: "success",
            })
            this.$router.push({path: "/main/${userId}"})
          }else{
            this.$message({
              message: "登录失败",
              type: "error",
            })
          }
        })
        .catch((err) => {
          console.log(err);
        });
      }
    },
    register() {
      this.$router.push({ path: "/register" });
    },
  },
};
</script>