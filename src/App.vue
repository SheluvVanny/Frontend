<template>
  <div id="app" class="container">
    <h1 class="text-center my-4">Online Lesson Booking</h1>
    <LessonList
      :cart="cart"
      :remaining-space="space"
      @add-to-cart="addToCart"
    />

    <!-- Shopping Cart Button -->
    <button
      class="btn btn-primary shopping-cart-button"
      data-bs-toggle="modal"
      data-bs-target="#shoppingCartModal"
    >
      CART
    </button>

    <!-- Shopping Cart Modal -->
    <div
      class="modal fade"
      id="shoppingCartModal"
      tabindex="-1"
      aria-labelledby="shoppingCartModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="shoppingCartModalLabel">Booked Lessons</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <ShoppingCart :cart="cart" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import LessonList from "./components/LessonList.vue";
import ShoppingCart from "./components/ShoppingCart.vue";

export default {
  components: {
    LessonList,
    ShoppingCart,
  },
  data() {
    return {
      cart: [],
      totalSpace: 15, // Maximum booking capacity
      space: 15, // Remaining booking space
    };
  },
  methods: {
    addToCart(lesson) {
  if (this.space > 0) {
    const cartLesson = this.cart.find((l) => l.id === lesson.id);
    if (cartLesson) {
      cartLesson.quantity++;
    } else {
      this.cart.push({ ...lesson, quantity: 1 });
    }
    this.space--; // Reduce remaining space by 1
  }
}

  },
};
</script>

<style>

.shopping-cart-button {
  position: fixed; 
  bottom: 20px; 
  right: 20px; 
  z-index: 1000; 
  border-radius: 50%; 
  width: 60px; 
  height: 60px; 
  display: flex;
  justify-content: center;
  align-items: center;
}

</style>

