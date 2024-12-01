<template>
  <div>
    <!-- Display Cart Items -->
    <ul v-if="cart.length > 0" class="list-group">
      <li
        v-for="item in cart"
        :key="item.id"
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        {{ item.subject }} ({{ item.quantity }})
        <button @click="removeFromCart(item)" class="btn btn-danger btn-sm">
          Remove
        </button>
      </li>
    </ul>
    <p v-else>Your cart is empty!</p>

    <!-- Checkout Form -->
    <div v-if="cart.length > 0" class="mt-4">
      <h6>Checkout</h6>
      <form @submit.prevent="checkout">
        <div class="mb-3">
          <label for="name" class="form-label">Name:</label>
          <input type="text" id="name" class="form-control" v-model="name" />
        </div>
        <div class="mb-3">
          <label for="phone" class="form-label">Phone:</label>
          <input type="text" id="phone" class="form-control" v-model="phone" />
        </div>
        <button type="submit" class="btn btn-success" :disabled="!isFormValid">
          Checkout
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { BASE_URL } from "../config.js";

export default {
  props: ["cart"],
  data() {
    return {
      name: "",
      phone: "",
    };
  },
  computed: {
    isFormValid() {
      return this.name.trim() && this.phone.trim();
    },
  },
  methods: {
    removeFromCart(item) {
      this.$emit("remove-item", item);
    },
    async checkout() {
      if (this.isFormValid) {
        const order = {
          name: this.name,
          phone: this.phone,
          items: this.cart,
        };
        try {
          const response = await fetch(`${BASE_URL}/orders`, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(order),
          });
          if (!response.ok) throw new Error("Checkout failed");
          alert("Order submitted successfully!");
          this.$emit("clear-cart");
        } catch (error) {
          console.error("Error during checkout:", error);
        }
      } else {
        alert("Please fill out the form correctly");
      }
    },
  },
};
</script>



