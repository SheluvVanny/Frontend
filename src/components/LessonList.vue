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
      lessons: [
                { id: 1, subject: "Algebra", location: "Canada Water", price: 50, availability: 10, image: "https://cdn.pixabay.com/photo/2016/06/13/07/59/pi-1453836_1280.jpg" },
        { id: 2, subject: "Fitness", location: "Wembley", price: 40, availability: 8, image: "https://cdn.pixabay.com/photo/2017/02/01/13/53/analysis-2030265_1280.jpg" },
        { id: 3, subject: "History", location: "Hendon", price: 30, availability: 5, image: "https://cdn.pixabay.com/photo/2010/12/01/space-shuttle-774_1280.jpg" },
        { id: 4, subject: "English", location: "Mayfair", price: 25, availability: 7, image: "https://cdn.pixabay.com/photo/2017/01/12/06/53/english-1973908_1280.png" },
        { id: 5, subject: "Biology", location: "Mayfair", price: 60, availability: 7, image: "https://cdn.pixabay.com/photo/2017/11/11/11/30/chemistry-2938901_1280.jpg" },
        { id: 6, subject: "Chemistry", location: "Hendon", price: 45, availability: 9, image: "https://media.istockphoto.com/id/1397013091/photo/laboratory-test-tube-glassware-with-pipette-dropper-and-flask.jpg?s=1024x1024&w=is&k=20&c=NbvMXh8VaCsW0JM2mD8dxOM5AryxP_VKQTZ0bcuj2uA=" },
        { id: 7, subject: "Physics", location: "Wembley", price: 35, availability: 6, image: "https://cdn.pixabay.com/photo/2018/12/12/17/08/physics-3871216_1280.jpg" },
        { id: 8, subject: "Art", location: "Canada Water", price: 20, availability: 10, image: "https://cdn.pixabay.com/photo/2016/12/15/20/21/texture-1909992_1280.jpg" },
        { id: 9, subject: "Music", location: "London Bridge", price: 55, availability: 9, image: "https://cdn.pixabay.com/photo/2018/03/12/22/15/music-notes-3221097_1280.jpg" },
        { id: 10, subject: "Geography", location: "London Bridge", price: 50, availability: 5, image: "https://cdn.pixabay.com/photo/2014/12/27/15/31/camera-581126_1280.jpg" },
        { id: 11, subject: "Programming", location: "Oxford Circus", price: 70, availability: 5, image: "https://cdn.pixabay.com/photo/2016/11/18/13/03/apple-1834328_1280.jpg" },
        { id: 12, subject: "Economics", location: "Oxford Circus", price: 65, availability: 2, image: "https://cdn.pixabay.com/photo/2014/10/23/10/10/dollars-499481_1280.jpg" },
      ],
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
};
</script>

<style>
.lesson-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}
</style>