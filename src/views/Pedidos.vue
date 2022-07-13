<template>
  <div class="container">

    <Dashboard />
    
  </div>
</template>

<script>
import Dashboard from "../components/Dashboard.vue";

export default {
  name: "Pedidos",
  components: {
    Dashboard
  }
}
</script>

<style>
  .container {
    height: 100vh;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>