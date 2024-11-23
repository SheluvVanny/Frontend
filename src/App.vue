<template>
  <div id="app" class="container">
    <h1 class="text-center my-4">Online Clothing Shop</h1>
    <ItemsList @add-to-cart="addToCart" :cart="cart" />

    <!-- Button to Open Shopping Cart Modal -->
    <button class="btn btn-primary mt-3" data-bs-toggle="modal" data-bs-target="#shoppingCartModal">
      Shopping Cart
    </button>

    <!-- Shopping Cart Modal -->
    <div class="modal fade" id="shoppingCartModal" tabindex="-1" aria-labelledby="shoppingCartModalLabel"
      aria-hidden="true">
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="shoppingCartModalLabel">Shopping Cart</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <!-- Shopping Cart Component -->
            <ShoppingCart :cart="cart" @remove-item="removeFromCart" @checkout="handleCheckout" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ItemsList from "./components/ItemsList.vue";
import ShoppingCart from "./components/ShoppingCart.vue";

export default {
  components: {
    ItemsList,
    ShoppingCart,
  },
  data() {
    return {
      cart: [],
    };
  },
  methods: {
    addToCart(item) {
      const cartItem = this.cart.find((i) => i.id === item.id);
      if (cartItem) cartItem.quantity++;
      else this.cart.push({ ...item, quantity: 1 });
    },
    removeFromCart(item) {
      const index = this.cart.indexOf(item);
      if (index > -1) {
        this.cart[index].availability += item.quantity;
        this.cart.splice(index, 1);
      }
    },
    handleCheckout(details) {
      console.log("Order Submitted", details);
      this.cart = []; // Clear cart after checkout
    },
  },
};
</script>


