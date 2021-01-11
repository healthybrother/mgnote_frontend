<template>
  <div class="register_page">
    <p style="font-size: 40px">MG Note</p>
    <el-row type="flex" align="middle" justify="center">
      <el-col :span="10">
        <el-card shadow="always">
          <el-form :model="infoForm" status-icon :rules="rules" ref="infoForm">
            <el-form-item
              label="用户名"
              :rules="[
                { required: true, message: '用户名不能为空', trigger: 'blur' },
              ]"
              prop="userName"
            >
              <el-input
                v-model="infoForm.userName"
                clearable
                placeholder="请输入用户名"
                autocomplete="off"
              ></el-input>
            </el-form-item>
            <el-form-item
              label="邮箱"
              prop="mail"
              :rules="[
                {
                  required: true,
                  message: '邮箱地址不能为空',
                  trigger: 'blur',
                },
                {
                  type: 'email',
                  message: '请输入正确的邮箱地址',
                  tirgger: ['blur', 'change'],
                },
              ]"
            >
              <el-input
                v-model="infoForm.mail"
                clearable
                placeholder="请输入邮箱或用户名"
                autocomplete="off"
              ></el-input>
            </el-form-item>
            <el-form-item label="密码" prop="pass">
              <el-input
                v-model="infoForm.password"
                type="password"
                placeholder="请输入密码"
                autocomplete="off"
              ></el-input>
            </el-form-item>
            <el-form-item label="确认密码" prop="checkPass">
              <el-input
                v-model="infoForm.rePassword"
                type="password"
                placeholder="请确认密码"
                autocomplete="off"
              ></el-input>
            </el-form-item>
          </el-form>

          <el-button
            class="register_btn"
            @click="register('infoForm')"
            type="primary"
            >注册</el-button
          >
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    var validatePass = (rule, value, callback) => {
      if (this.infoForm.password === "") {
        callback(new Error("请输入密码"));
      } else {
        if (this.infoForm.rePassword !== "") {
          this.$refs.infoForm.validateField("checkPass");
        }
        callback();
      }
    };
    var validatePass2 = (rule, value, callback) => {
      if (this.infoForm.rePassword === "") {
        callback(new Error("请再次输入密码"));
      } else if (this.infoForm.rePassword !== this.infoForm.password) {
        callback(new Error("两次输入密码不一致"));
      } else {
        callback();
      }
    };
    return {
      infoForm: {
        userName: "",
        mail: "",
        password: "",
        rePassword: "",
      },
      rules: {
        pass: [{ validator: validatePass, trigger: "blur" }],
        checkPass: [{ validator: validatePass2, trigger: "blur" }],
      },
    };
  },
  methods: {
    register(aForm) {
      this.$refs[aForm].validate((valid) => {
        if (valid) {
          this.axios({
            method: "POST",
            url: "/user/signup",
            data: {
              userName: this.infoForm.userName,
              mail: this.infoForm.mail,
              password: this.infoForm.password,
            },
          })
            .then((res) => {
              console.log(res);
              if (res.status == 201) {
                this.$message({
                  message: "注册成功",
                  type: "success",
                });
                let userId = res.data.id;
                this.$router.push({ path: "/main/${userId}" });
              } else {
                this.$message({
                  message: "注册失败",
                  type: "error",
                });
              }
            })
            .catch((err) => {
              console.log(err);
            });
        } else {
          console.log("错误");
        }
      });
    },
  },
};
</script>