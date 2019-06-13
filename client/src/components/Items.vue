
<template>
  <div>
    <form class="w-full max-w-sm">
        <Modal v-if="modalOpen" @close-modal="modalOpen = false" :item="modalItem"/>
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
        <Product class="inline-block p-4" v-for="item in results" :key="item.id" :item="item"
        @click.native="handleModalOpen(item)"/>
        <paginate
          :page-count="lastPage"
          :page-range="3"
          :margin-pages="2"
          :click-handler="clickCallback"
          :prev-text="'Prev'"
          :next-text="'Next'"
          :container-class="'pagination'"
          :page-class="'page-item'"
          :next-class="'none'"
          :prev-class="'none'"
          :active-class="'page-item active'"
        ></paginate>
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
import Modal from "./Modal";
import debounce from "lodash.debounce";
import axios from "axios";
import { constants } from "fs";
import Paginate from "vuejs-paginate";

const API = "http://localhost:3005/products";

export default {
  name: "Items",
  data() {
    return {
      searchValue: "",
      results: [],
      inCart: [],
      links: [],
      linkString: "",
      lastPage: 0,
      paginationPage: 1,
      modalOpen: false,
      modalItem: null,
    };
  },
  mounted() {
    this.paginationPage = 1;
    this.getPosts();
  },
  methods: {
    handleInput: debounce(function() {
      this.paginationPage = 1;
      this.getPosts();
    }, 500),

    handleModalOpen(item) {
      this.modalOpen = true;
      this.modalItem = item;
    },

    getPosts() {
      axios
        .get(`${API}?q=${this.searchValue}&_limit=12&_page=${this.paginationPage}`)
        .then(response => {
          this.results = response.data;
          this.linkString = response.headers.link;
          this.setLastPage();
        })
        .catch(error => {
          console.log(error);
        });
    },

    setLastPage() {
      let iStart = 0;
      let iStop = 0;

      for (let i = 0; i < this.linkString.length; i++) {
        if (this.linkString[i] === ">") {
          iStop = i;
        }
        if (this.linkString[i] === "=" && this.linkString[i - 1] === "e") {
          iStart = i + 1;
        }
      }
      this.lastPage = Number(this.linkString.slice(iStart, iStop));
    },

    clickCallback(pageNum) {
      this.paginationPage = pageNum;
      this.getPosts();
    }
  },
  components: {
    Product,
    Cart,
    Modal,
    Paginate
  }
};
</script>

<style lang="scss">
form {
  margin: 0 auto;
}
.pagination {
  margin: 1rem;
  list-style: none;
}
.none {
  display: none;
}

.page-item {
  box-sizing: border-box;
  display: inline-block;
      margin: 0.5rem 0;

  a {
    margin: 0.25rem;
    padding: 0.5rem 1rem;
    background-color: #e6e6e6;
    border: 1px solid #b9b9b9;
    border-radius: 2px;
  }
}
.active {
  a {
    background-color: #48bb78;
    color: #ffffff;
  }
}
</style>
