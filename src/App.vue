<template>
  <Navbar :logo="logo_src" :alt="app_name" />
  <router-view/>
</template>

<script>
import Navbar from './components/Navbar.vue';

export default {
  components: {
    Navbar,
  },
  data() {
    return {
      logo_src: "/img/logo.png",
      app_name: "Make your order"
    }
  }
}
</script>


<style>
  * {
    padding: 0;
    margin: 0;
  }

  .main-container {
    margin: 50px;
    min-height: 681px;
  }

  h1 {
    text-align: center;
    font-size: 42px;
    margin-bottom: 30px;
    color: #222;
  }
</style>
