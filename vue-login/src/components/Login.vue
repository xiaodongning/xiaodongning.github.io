<template>
  <div>
    <div class="bg-blur">
      <div class="mask"></div>
    </div>
    <el-container class="login">
      <el-header>
        <h1 class="u-center" style="color:#fff">用户登录</h1>
      </el-header>
      <el-main>
        <el-row type="flex" class="row-bg" justify="space-around">
          <el-col :span="6">
            <el-card class="box-card">
              <div slot="header" class="clearfix">
                <span>用户登录</span>
                <i style="float:right" class="el-icon-close"></i>
              </div>
              <el-form :model="formLogin" :rules="rules" ref="formLogin" label-width="80px">
                <el-form-item label="用户名" prop="name">
                  <el-input v-model="formLogin.name"></el-input>
                </el-form-item>
                <el-form-item label="密码" prop="password">
                  <el-input type="password" v-model="formLogin.password"></el-input>
                </el-form-item>
                <el-form-item>
                  <el-button type="primary" @click="submitForm('formLogin')" :loading="loading">登录</el-button>
                  <el-button>重置</el-button>
                </el-form-item>
              </el-form>
            </el-card>
          </el-col>
        </el-row>

      </el-main>
    </el-container>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Login",
  data() {
    return {
      formLogin: {
        name: "",
        password: ""
      },
      loading: false,
      rules: {
        name: [
          {
            required: true,
            message: "请填写用户名",
            trigger: "blur"
          }
        ],
        password: [
          {
            required: true,
            message: "请填写密码"
          }
        ]
      }
    };
  },
  mounted() {
    axios.interceptors.request.use(
      config => {
        this.loading = true;
        return config;
      },
      error => {
        return Promise.reject(error);
      }
    );
    // http响应拦截器
    axios.interceptors.response.use(
      data => {
        this.loading = false;
        return data;
      },
      error => {
        return Promise.reject(error);
      }
    );
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.login();
        } else {
          alert("失败");
        }
      });
    },
    login() {
      axios
        .post("https://neteaseapi.herokuapp.com/userinfo", {
          username: this.formLogin.name,
          password: this.formLogin.password
        })
        .then(res => {
          if (res) {
            let data = res.data;
            if (data.code == 200) {
              let userInfo = {};
              userInfo.name = data.data.username;
              userInfo.avatar = data.data.avatar;
              userInfo = JSON.stringify(userInfo);
              localStorage.setItem("session", userInfo);
              this.$router.push("/user_info");
            } else {
              this.$message({
                showClose: true,
                message: data.msg,
                type: "error"
              });
            }
          }
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  beforeRouteEnter(to, from, next) {
    console.log(localStorage.getItem("session"));
    if (localStorage.getItem("session")) {
      // next(vm => {
      //   vm.$router.push("/user_info");
      // });
    } else {
      next(true);
    }
    // next(true);
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.login {
  position: fixed;
  width: 100%;
}

.bg-blur {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: url(../assets/bg.jpg) no-repeat 0 0 / 100%;
}

.mask {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.5);
}
</style>
<style>

</style>
