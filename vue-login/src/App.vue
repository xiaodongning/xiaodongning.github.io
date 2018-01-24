<template>
  <div class="container" id="app">
    <transition name="fade">
      <keep-alive>
        <router-view/>
      </keep-alive>
    </transition>
  </div>
</template>
<script>
  import axios from 'axios';

  export default {
    name: "app",
    data() {
      return {
        msg: "",
        avatar: require("./assets/logo.png")
      };
    },

    mounted() {

    }
  };

</script>

<style>
  .u-center {
    text-align: center;
  }

  .container{
    overflow: hidden;
  }
/* transition animate */
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s
}
.fade-enter, .fade-leave-active {
  opacity: 0
}


.slide-enter-active {
  transition: all .3s ease;
}
.slide-leave-active {
  /* transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0); */
}
.slide-enter, .slide-leave-active {
  transform: translateX(10px);
  opacity: 0;
}

</style>
