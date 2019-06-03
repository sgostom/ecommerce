<template>
  <div>
    <h1>{{ msg }}</h1>
    <form class="w-full max-w-sm">
      <br>
      <label for="search">Search products:</label>
      <div class="flex items-center border-b border-b-2 border-teal-500 py-2">
        <br>
        <input
          id="search"
          class="appearance-none bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
          name="search"
          v-model="searchValue"
          @input="handleInput"
        >
      </div>
    </form>
    <br>

    <Product class="inline-block p-4"
      v-for="item in results"
      :key="item.id"
      :itemId="item.id"
      :imageUrl="item.images.primary.large"
      :description="item.general.description"
      :name="item.general.name"
    />

  </div>
</template>

<script>
import Product from './Product';
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
      results: []
    };
  },
  created(){
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
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
form {
  margin: 0 auto;
}
</style>
