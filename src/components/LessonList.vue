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
        </select>
      </div>
    </div>

    <!-- Lesson Cards -->
    <div class="row">
      <div v-for="lesson in sortedLessons" :key="lesson._id" class="col-md-4">
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
import { BASE_URL } from "../config.js";

export default {
  props: ["cart", "remainingSpace"],
  data() {
    return {
      lessons: [],
      sortBy: "subject",
    };
  },
  computed: {
    sortedLessons() {
      return [...this.lessons].sort((a, b) => {
        if (this.sortBy === "price") return a.price - b.price;
        return a[this.sortBy].localeCompare(b[this.sortBy]);
      });
    },
  },
  methods: {
    async fetchLessons() {
      try {
        const response = await fetch(`${BASE_URL}/lessons`);
        if (!response.ok) {
          throw new Error("Failed to fetch lessons");
        }
        this.lessons = await response.json();
      } catch (error) {
        console.error("Error fetching lessons:", error);
      }
    },
    addToCart(lesson) {
      this.$emit("add-to-cart", lesson);
      lesson.availability--;
    },
  },
  created() {
    this.fetchLessons();
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


