<template>
  <div>
    <h1>{{ msg }}</h1>


    <form class="w-full max-w-sm">
      <br>
      <label for="search">Search</label>
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
    <ul>
      <li v-for="item in results" :key="item.id">
        <p>{{item.general.name}}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import debounce from 'lodash.debounce';
import axios from 'axios';
const API =  'http://localhost:3005/products';


export default {
  name: "Items",
  props: {
    msg: String
  },
  data() {
    return {
      searchValue: '',
      results: [],
    };
  },
  methods: {
    handleInput: debounce(function () {
      axios.get(API)
      .then((response) => {
        console.log(response.data);
        this.results = response.data;
      })
      .catch((error) => {
        console.log(error);
      });
    },500)
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
form {
  margin: 0 auto;
}
</style>
