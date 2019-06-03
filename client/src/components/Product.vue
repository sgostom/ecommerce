<template>
  <div class="max-w-sm rounded overflow-hidden shadow-lg">
    <img class="w-full" :src="imageUrl" alt="Sunset in the mountains">
    <div class="px-6 py-4">
      <span
        class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 my-4"
      >#{{itemId}}</span>
      <div class="font-bold text-l mb-2">{{name}}</div>
    </div>
<button class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded-full">
  Add to cart
</button>
    <div class="px-6 py-4"></div>
  </div>
</template>

<script>
export default {
  name: "Product",
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  },
  props: {
    imageUrl: {
      type: String,
      default: ""
    },
    name: {
      type: String
    },
    description: {
      type: String
    },
    itemId: {
      type: String
    }
  }
};
</script>

<style>
</style>
