<template>
  <div class="outerWrapper" @click="$emit('close-modal')" >
    <div class>
      <div class="max-w-sm rounded overflow-auto shadow-lg innerWrapper" v-on:click.stop>
        <div class="close" @click="$emit('close-modal')"/>

        <div class="sm:flex w-full sm:items-center px-6 py-4">
          <img
            class="block h-26 sm:h-24 rounded-full mx-auto mb-4 sm:mb-0 sm:mr-4 sm:ml-0"
            :src="item.images.primary.large"
            alt
          >
          <div class="text-center sm:text-left sm:flex-grow mb-6">
              <p class="text-xl leading-tight mb-2">{{item.general.name}}</p>
              <p class="text-l leading-tight mb-4">{{item.brand.name}}</p>
              <div class="flex flex-warp mb-6">
                <span
                  class="inline-block bg-gray-200 rounded-full px-3 py-2 text-sm font-semibold text-gray-700 mr-2"
                >#{{item.id}}</span>
                <button
                  @click="pass_to_cart($event,item)"
                  class="bg-green-500 hover:bg-green-700 text-white font-bold px-3 py-2 rounded-full"
                >Add to cart</button>
              </div>
              <p class="text-m" v-html="item.general.description"></p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  data() {
    return {
      itemDescription: null
    };
  },
  props: ["item"],

  methods: {
    pass_to_cart: function(e, single_product_object) {
      this.$root.$emit("add_to_cart", single_product_object);
    }
  }
};
</script>

<style scoped lang="scss">
.outerWrapper {
  background: #9b9b9b5b;
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
}
.innerWrapper {
  position: relative;
  background: #ffffff;
  display: flex;
  max-width: 800px;
  margin: 5rem auto;
  height: calc(100vh - 10rem);
  padding: 30px;
}

.close {
  position: absolute;
  right: 0;
  top: 0;
  padding: 30px;
  width: 30px;
  height: 30px;
  cursor: pointer;

  &::before,
  &::after {
    position: absolute;
    content: "";
    top: 30px;
    right: 20px;
    width: 20px;
    height: 2px;
    background: black;
    display: block;
  }
  &::after {
    transform: rotate(45deg);
  }
  &::before {
    transform: rotate(-45deg);
  }
}
</style>