<template>
  <el-row class="user-info">
    <el-col :span="8" :offset="8">
      <el-card :body-style="{ padding: '0px' }">
        <img :src="avatar" width="100%" class="image">
        <div style="padding: 14px;">
          <span>Hello! {{ username }}</span>
          <div class="bottom clearfix">
            <time class="time">{{ currentDate | timeFormat }}</time>
            <el-button type="text" @click="logout" class="button">退出</el-button>
          </div>
        </div>
      </el-card>
    </el-col>
  </el-row>
</template>

<script>
import { formatDate } from "@/assets/js/date.js";
export default {
  name: "UserInfo",
  data() {
    return {
      avatar: "",
      username: "",
      currentDate: new Date()
    };
  },
  methods: {
    logout: function() {

      localStorage.removeItem("session");
      this.$router.push("/login");
    }
  },
  filters: {
    timeFormat: function(value) {
      return formatDate(value, "yyyy-MM-dd hh:mm");
    }
  },
  beforeRouteEnter(to, from, next) {
    if (!localStorage.getItem("session")) {
      next(vm => {
        vm.$router.push("/login");
      });
    } else {
      next(true);
    }
  },
  mounted() {
    if (localStorage.getItem("session")) {
      let userInfo = JSON.parse(localStorage.getItem("session"));
      this.avatar = userInfo.avatar;
      this.username = userInfo.name;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.user-info{
  position: fixed;
  width: 100%;
}

body {
  background-color: #f9fafc;
  background-image:none;
}
</style>

<style>

</style>
