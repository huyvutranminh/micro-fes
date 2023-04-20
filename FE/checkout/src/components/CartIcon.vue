<template>
  <a class="icon-container" @click="navigateToCart">
    <i class="fa fa-shopping-bag" />
    <span>{{ totalItems }}</span>
  </a>
</template>
<script lang="ts">
import { useCartStore } from "@/store/cart";
import { storeToRefs } from "pinia";
import { defineComponent, nextTick } from "vue";
import globalStorage from "@/utils/loadStorage";


export default defineComponent({
  name: "CartIcon",
  props: ["navigate", "eventBus"],
  setup() {
    const cartStore = useCartStore();
    const { totalItems } = storeToRefs(cartStore);
    cartStore.load();
    return { cartStore, totalItems };
  },
  mounted() {
    this.$props.eventBus.on('addToCart', () => {
      this.cartStore.addItem()
    })
    document.body.addEventListener("addToCartCustom", () => this.cartStore.addItem());
  },
  methods: {
    navigateToCart() {
      this.navigate("/shopping-cart");
    },
  },
});
</script>
