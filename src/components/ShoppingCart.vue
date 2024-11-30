<template>
    <div>
        <!-- Display cart items -->
        <ul v-if="cart.length > 0" class="list-group">
            <li v-for="item in cart" :key="item.id"
                class="list-group-item d-flex justify-content-between align-items-center">
                <!-- Display lesson subject -->
                {{ item.subject }} ({{ item.quantity }} x ${{ item.price }})
                <button @click="removeFromCart(item)" class="btn btn-danger btn-sm">
                    Remove
                </button>
            </li>
        </ul>
        <p v-else>Your cart is empty!</p>

        <!-- Checkout form -->
        <div v-if="cart.length > 0" class="mt-4">
            <h6>Checkout</h6>
            <form @submit.prevent="checkout">
                <div class="mb-3">
                    <label for="name" class="form-label">Name:</label>
                    <input type="text" id="name" class="form-control" v-model="name" placeholder="Enter your name" />
                </div>
                <div class="mb-3">
                    <label for="phone" class="form-label">Phone:</label>
                    <input type="text" id="phone" class="form-control" v-model="phone"
                        placeholder="Enter your phone number" />
                </div>
                <button type="submit" class="btn btn-success" :disabled="!isFormValid">
                    Checkout
                </button>
            </form>
        </div>
    </div>
</template>

<script>
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
            const nameValid = /^[a-zA-Z\s]+$/.test(this.name);
            const phoneValid = /^[0-9]+$/.test(this.phone);
            return nameValid && phoneValid && this.cart.length > 0;
        },
    },
    methods: {
        removeFromCart(item) {
            this.$emit("remove-item", item);
        },
        async checkout() {
            if (!this.isFormValid) {
                alert("Invalid Form Details");
                return;
            }

            // Order object
            const orderData = {
                name: this.name,
                phone: this.phone,
                items: this.cart.map(item => ({
                    id: item.id,
                    subject: item.subject,
                    quantity: item.quantity,
                    price: item.price
                }))
            };

            try {
                const response = await fetch("http://localhost:5000/api/orders", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify(orderData),
                });

                if (!response.ok) {
                    throw new Error("Failed to submit order");
                }

                const data = await response.json();
                alert(`Order Submitted Successfully! Order ID: ${data.orderId}`);

                // Reset the form and clear the cart
                this.name = "";
                this.phone = "";
                this.$emit("clear-cart");
            } catch (error) {
                console.error("Error submitting order:", error);
                alert("Failed to submit order. Please try again.");
            }
        },
    },
};
</script>

<style scoped>
/* Styles for the cart */
.list-group-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
</style>


