<template>
  <div class="cart-side-bar">
    <label class="italic" for="search">Your cart:</label>

    <div class="container mx-auto" v-for="(item, index) in inCart" :key="index">
      <div
        class="border m-6 rounded-lg bg-white mx-auto max-w-sm shadow-lg rounded-lg overflow-hidden"
      >
        <div class="sm:flex sm:items-center px-6 py-4">
          <img
            class="block h-16 sm:h-24 rounded-full mx-auto mb-4 sm:mb-0 sm:mr-4 sm:ml-0"
            :src="item.images.primary.large"
            alt
          >
          <div class="text-center sm:text-left sm:flex-grow">
            <div class="mb-1">
              <p class="text-l leading-tight">{{item.general.name}}</p>
            </div>
            <div class="flex flex-wrap">
              <span
                class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm text-gray-700 mr-2 my-4"
              >#{{item.id}}</span>
              <span
                class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm text-gray-700 mr-2 my-4"
              >
                <input
                  type="number"
                  class="form-control bg-gray-200"
                  min="1"
                  max="100"
                  v-model="item.quantity"
                  @change="updateProductQuantity"
                >
              </span>

              <button
                @click="removeFromCart(item.id)"
                class="bg-green-500 hover:bg-green-700 text-white font-semibold py-2 px-4 rounded-full"
              >Remove from cart</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div v-if="inCart.length > 0">
      <button
        class="bg-green-500 hover:bg-green-700 text-white font-semibold py-2 px-4 rounded-full"
      >Check out</button>
    </div>
    <label v-else class="italic" for="search">Your cart is empty. Add some amazing things ;)</label>
  </div>
</template>
<script>
export default {
  props: ["products"],
  data: function() {
    return {
      inCart: []
    };
  },

  methods: {
    updateProductQuantity() {
      this.$forceUpdate();
      this.saveCart();
    },
    removeFromCart(id) {
      let index = this.findIndexById(id);
      this.inCart.splice(index, 1);
      this.saveCart();
    },
    findIndexById(id) {
      let products = this.inCart;
      let index = "";

      for (let i in products) {
        if (id == products[i].id) {
          index = i;
          break;
        }
      }
      return index;
    },
    saveCart: function() {
      let parsed = JSON.stringify(this.inCart);
      localStorage.setItem("cart", parsed);
    }
  },

  mounted() {
    if (localStorage.getItem("cart")) {
      try {
        this.inCart = JSON.parse(localStorage.getItem("cart"));
      } catch (e) {
        localStorage.removeItem("cart");
      }
    }

    this.$root.$on("add_to_cart", data => {
      let found = "";
      let index = "";

      for (let i in this.inCart) {
        if (data.id == this.inCart[i].id) {
          found = true;
          index = i;
          break;
        } else {
          found = false;
        }
      }

      if (found) {
        let new_value = parseInt(this.inCart[index].quantity) + 1;
        this.$set(this.inCart[index], "quantity", new_value);
        this.updateProductQuantity();
      } else {
        data.quantity = 1;
        this.inCart.push(data);
        this.saveCart();
      }
    });
  }
};
</script>


<style scoped lang="scss">
</style>


