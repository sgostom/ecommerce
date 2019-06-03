<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <Items msg="Welcome to Your  eCommerce Vue.js App"/>

  </div>
</template>

<script>
// @ is an alias to /src
import Items from "@/components/Items.vue";

export default {
  name: "home",
  components: {
    Items
  }
};
</script>


<style scoped lang="scss">
img {
  display: inline;
}
</style>