<template>
  <div>
    <h2>Available Lessons</h2>
    <!-- Sorting Options -->
    <div class="d-flex justify-content-between my-3">
      <div>
        <label>Sort By:</label>
        <select v-model="sortBy" class="form-select">
          <option value="subject">Subject</option>
          <option value="location">Location</option>
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

    <!-- Lesson Cards -->
    <div class="row">
      <div v-for="lesson in sortedLessons" :key="lesson.id" class="col-md-4">
        <div class="card mb-4">
          <img
            :src="lesson.image"
            class="card-img-top lesson-image"
            :alt="lesson.subject"
          />
          <div class="card-body">
            <h5 class="card-title">{{ lesson.subject }}</h5>
            <p class="card-text">
              Location: {{ lesson.location }} <br />
              Price: ${{ lesson.price }} <br />
              Availability: {{ lesson.availability }}
            </p>
            <!-- Add to Cart Button -->
            <button
              class="btn btn-primary"
              @click="addToCart(lesson)"
              :disabled="remainingSpace === 0 || lesson.availability === 0"
            >
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
  props: ["cart", "remainingSpace"],
  data() {
    return {
      lessons: [], // Initialize as empty
      sortBy: "subject",
      sortOrder: "asc",
    };
  },
  computed: {
    sortedLessons() {
      const sorted = [...this.lessons];
      sorted.sort((a, b) => {
        if (this.sortOrder === "asc") {
          return a[this.sortBy] > b[this.sortBy] ? 1 : -1;
        } else {
          return a[this.sortBy] < b[this.sortBy] ? 1 : -1;
        }
      });
      return sorted;
    },
  },
  methods: {
    async fetchLessons() {
      try {
        const response = await fetch("http://localhost:5000/api/lessons");
        if (!response.ok) throw new Error("Failed to fetch lessons");
        this.lessons = await response.json();
        console.log("Fetched lessons:", this.lessons); // Debug log
      } catch (error) {
        console.error("Error fetching lessons:", error);
      }
    },
    addToCart(lesson) {
      if (this.remainingSpace > 0 && lesson.availability > 0) {
        this.$emit("add-to-cart", lesson);
        lesson.availability--; // Decrease availability
      }
    },
    toggleSortOrder() {
      this.sortOrder = this.sortOrder === "asc" ? "desc" : "asc";
    },
  },
  mounted() {
    this.fetchLessons(); // Fetch lessons on component mount
  },
};
</script>

<style scoped>
.lesson-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}
</style>

