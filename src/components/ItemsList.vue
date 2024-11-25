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
                { id: 1, name: "Shirt", category: "Clothes", price: 300, availability: 10, image: "https://i5.walmartimages.com/asr/c33bb859-28b0-48cb-a10c-f875059fd86a_1.d6d7413a3f0147ff6abfebc0a824b8ac.jpeg" },
                { id: 2, name: "Jeans", category: "Clothes", price: 500, availability: 7, image: "https://i.pinimg.com/736x/ac/c6/bd/acc6bdce85c05463b42927018cde2589.jpg"},
                { id: 3, name: "Sneaker", category: "Shoes", price: 1000, availability: 6, image: "https://i.pinimg.com/originals/4e/cf/1a/4ecf1adaefa90545caa7864771961cdf.jpg" },
                { id: 4, name: "Boots", category: "Shoes", price: 1200, availability: 5, image: "https://th.bing.com/th?id=OPEC.FRo1SXq%2b1vmNaA474C474&o=5&pid=21.1&w=160&h=150&dpr=2" },
                { id: 5, name: "Baseball-Cap", category: "Accessories", price: 200, availability: 8, image: "https://th.bing.com/th/id/OIP.1LWrME-VN2movm2LUgBwCQHaJ4?w=128&h=180&c=7&r=0&o=5&dpr=2&pid=1.7" },
                { id: 6, name: "Bracelet", category: "Accessories", price: 450, availability: 12, image: "https://news.thediamondstore.co.uk/wp-content/uploads/2021/10/NDUD4223Uh.jpg" },
                { id: 7, name: "Hoodie", category: "Clothes", price: 600, availability: 13, image: "https://i.pinimg.com/originals/96/7b/f7/967bf7c2a1347ec3bbbfa4e28e467eb4.jpg" },
                { id: 8, name: "Necklace", category: "Accessories", price: 150, availability: 11, image: "https://i.pinimg.com/originals/a9/b6/32/a9b63204a4cd260513a97d44c21eef0e.jpg" },
                { id: 9, name: "Loafers", category: "Shoes", price: 1500, availability: 9, image: "https://th.bing.com/th/id/OIP.vJEYHp0xX5a3RjYSlZ2sygHaHu?w=207&h=215&c=7&r=0&o=5&dpr=2&pid=1.7" },
                { id: 10, name: "Ring", category: "Accessories", price: 900, availability: 14, image: "https://th.bing.com/th/id/R.809fca3db36fc99968f42cc69a1e8bba?rik=dkDDv%2fRMkiXLSg&riu=http%3a%2f%2fwwcdn.weddingwire.com%2fvendor%2f1_5000%2f391%2fthumbnails%2f800x800_1375214920342-dw56075.jpg&ehk=FAJ7KXPa13H9SGzDNn7Ee%2fIF2Sw7YhfviICjqPGn76k%3d&risl=&pid=ImgRaw&r=0" },
                { id: 11, name: "Dressy-Pants", category: "Clothes", price: 800, availability: 15, image:"https://cdn11.bigcommerce.com/s-wlu989/images/stencil/2560w/products/310/646/a20791a146bb50539792e2_l__56791.1419553740.jpg?c=2"},
                { id: 12, name: "Slides", category: "Shoes", price: 400, availability: 16, image: "https://th.bing.com/th/id/OIP.p6Yl0eQciVTOlIyazfAwCQHaIa?w=183&h=208&c=7&r=0&o=5&dpr=2&pid=1.7" },
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



