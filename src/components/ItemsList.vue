<template>
    <div>
        <h2>Products</h2>
        <div class="d-flex justify-content-between my-3">
            <div>
                <label>Category:</label>
                <select v-model="categoryFilter" @change="filterByCategory" class="form-select">
                    <option value="">All</option>
                    <option value="Clothes">Clothes</option>
                    <option value="Shoes">Shoes</option>
                    <option value="Accessories">Accessories</option>
                </select>
            </div>
            <div>
                <label>Sort By:</label>
                <select v-model="sortBy" @change="sortItems" class="form-select">
                    <option value="name">Name</option>
                    <option value="price">Price</option>
                    <option value="availability">Availability</option>
                </select>
            </div>
            <div>
                <button @click="toggleSortOrder" class="btn btn-secondary">
                    {{ sortOrder === 'asc' ? 'Ascending' : 'Descending' }}
                </button>
            </div>
        </div>

        <div class="row">
            <div v-for="item in filteredItems" :key="item.id" class="col-md-4">
                <div class="card mb-4">
                    <img :src="item.image" class="card-img-top" :alt="item.name" />
                    <div class="card-body">
                        <h5 class="card-title">{{ item.name }}</h5>
                        <p class="card-text">
                            Category: {{ item.category }} <br />
                            Price: ${{ item.price }} <br />
                            Availability: {{ item.availability }}
                        </p>
                        <button @click="addToCart(item)" class="btn btn-primary" :disabled="item.availability === 0">
                            Add to Cart
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ["cart"],
    data() {
        return {
            items: [
                { id: 1, name: "T-Shirt", category: "Clothes", price: 30, availability: 10, image: "tshirt.jpg" },
                { id: 2, name: "Jeans", category: "Clothes", price: 50, availability: 5, image: "jeans.jpg" },
                { id: 3, name: "Sneakers", category: "Shoes", price: 100, availability: 2, image: "sneakers.jpg" },
                { id: 4, name: "Boots", category: "Shoes", price: 120, availability: 3, image: "boots.jpg" },
                { id: 5, name: "Hat", category: "Accessories", price: 20, availability: 8, image: "hat.jpg" },
                { id: 6, name: "Scarf", category: "Accessories", price: 15, availability: 12, image: "scarf.jpg" },
            ],
            categoryFilter: "",
            sortBy: "name",
            sortOrder: "asc",
        };
    },
    computed: {
        filteredItems() {
            let filtered = this.items;
            if (this.categoryFilter) {
                filtered = filtered.filter((item) => item.category === this.categoryFilter);
            }
            return filtered;
        },
    },
    methods: {
        addToCart(item) {
            if (item.availability > 0) {
                this.$emit("add-to-cart", item);
                item.availability--;
            }
        },
        sortItems() {
            this.items.sort((a, b) => {
                if (this.sortOrder === "asc") return a[this.sortBy] > b[this.sortBy] ? 1 : -1;
                return a[this.sortBy] < b[this.sortBy] ? 1 : -1;
            });
        },
        toggleSortOrder() {
            this.sortOrder = this.sortOrder === "asc" ? "desc" : "asc";
            this.sortItems();
        },
        filterByCategory() {
            this.sortItems();
        },
    },
};
</script>


