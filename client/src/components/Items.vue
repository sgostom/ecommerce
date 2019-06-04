<template>
  <div>
    <form class="w-full max-w-sm">
      <label class="italic" for="search">Search products:</label>
      <div class="flex items-center border-b border-b-2 border-teal-500 py-2">
        <input
          id="search"
          class="appearance-none bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
          name="search"
          v-model="searchValue"
          @input="handleInput"
        >
      </div>
    </form>

    <div class="flex flex-wrap overflow-hidden">
      <div class="my-2 px-2 w-full overflow-hidden xl:w-2/3">
        <Product
          class="inline-block p-4"
          v-for="item in results"
          :key="item.id"
          :item="item"
        />
      </div>

      <div class="my-2 px-2 w-full overflow-hidden xl:w-1/3">
        <Cart/>
      </div>
    </div>
  </div>
</template>

<script>
import Product from "./Product";
import Cart from "./Cart";
import debounce from "lodash.debounce";
import axios from "axios";
const API = "http://localhost:3005/products";

export default {
  name: "Items",
  props: {
    msg: String
  },
  data() {
    return {
      searchValue: "",
      results: [],
      inCart: []
    };
  },
  created() {
    axios
      .get(`${API}?_limit=12`)
      .then(response => {
        console.log(response.data);
        this.results = response.data;
      })
      .catch(error => {
        console.log(error);
      });
  },
  methods: {
    handleInput: debounce(function() {
      axios
        .get(`${API}?q=${this.searchValue}`)
        .then(response => {
          console.log(response.data);
          this.results = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    }, 500)
  },
  components: {
    Product,
    Cart
  }
};
</script>

<style scoped lang="scss">
form {
  margin: 0 auto;
}
</style>
