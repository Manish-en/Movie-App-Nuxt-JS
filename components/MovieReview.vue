<template>
    <div v-if="reviews.length != 0" class="review-container">
      <div class="review-header">
        <div class="review-title">Reviews</div>
        <div class="review-navigation">
          <i 
            class="material-icons navbtn" 
            @click="decrement" 
            :class="page == 1 ? 'disable' : ''"
          >keyboard_double_arrow_left</i>
          <span class="page-number"> {{ page }} </span>
          <i 
            class="material-icons navbtn" 
            @click="increment"
            :class="reviews.length <= 10 ? 'disable' : ''"
          >keyboard_double_arrow_right</i>
        </div>
      </div>
      <div class="reviews-list">
        <div v-for="review in reviews" :key="review.id" class="review-card"> 
          <div class="review-author">Written by: {{ review.author }}</div>
          <div class="review-content">{{ review.content }}</div>
        </div>
      </div>
    </div>
    <div v-else class="review-container">
      <p class="no-reviews">No reviews</p>
    </div>
</template>
  

<script setup>
    
    const id = defineProps(['id']);

    let reviews = ref([]);
    let page = ref(1);


    async function getReviews(){
        let reviewsUrl = `https://api.themoviedb.org/3/movie/${id.id}/reviews?api_key=bb5c9a25161603cb7d1205e55e4cbe88&language=en-US&page=${page.value}`


        let reviewResult = await fetch(reviewsUrl)
        .then((response) => response.json())
        
        reviews.value = await reviewResult.results;
    }
    function increment(){
        page.value++;
        getReviews()
    }
    function decrement(){
        page.value--;
        getReviews()    
    }

    await getReviews();

</script>

<style scoped>
/* Main container for the reviews, fitting within the reserved 30% width */
.review-container {
  width: 100%;
  background-color: #1c1c1c; /* Matches the dark theme */
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
  color: #ecf0f1; /* Light text color for readability */
}

/* Header section with title and navigation */
.review-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.review-title {
  font-family: 'Sigmar', cursive;
  font-size: 1.8rem;
  color: #ff4500; /* Highlighted color for the title */
}

.review-navigation {
  display: flex;
  align-items: center;
}

.navbtn {
  color: #ecf0f1;
  margin: 0 0.5rem;
  transition: color 0.3s, transform 0.3s;
}

.navbtn:hover {
  color: #1abc9c; /* Aqua color on hover for a fresh look */
  cursor: pointer;
  transform: scale(1.2);
}

.disable {
  pointer-events: none;
  opacity: 0.3;
}

.page-number {
  font-size: 1.5rem;
  color: #ecf0f1;
}

/* List of reviews with a scrollable container */
.reviews-list {
  max-height: 120vh;
  overflow-y: auto;
}

.reviews-list::-webkit-scrollbar {
  display: none;
}

/* Individual review card styling */
.review-card {
  background-color: #2c2c2c; /* Slightly lighter than the background */
  padding: 1rem;
  margin-bottom: 1rem;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}

.review-author {
  font-weight: bold;
  color: #ff4500;
  margin-bottom: 0.5rem;
}

.review-content {
  color: #ecf0f1;
  font-size: 1.1rem;
  line-height: 1.5;
  max-height: 150px; /* Limit the height of the review content */
  overflow-y: hidden; /* Hide overflow by default */
  text-overflow: ellipsis;
  position: relative;
  transition: max-height 0.3s ease-in-out;
}

/* Show scroll bar only on hover */
.review-card:hover .review-content {
  overflow-y: auto; /* Make it scrollable when hovered */
  max-height: 150px; /* Keeps the height fixed, but allows scrolling */
}

.review-content::-webkit-scrollbar {
  width: 6px;
}

.review-content::-webkit-scrollbar-thumb {
  background-color: #555;
  border-radius: 10px;
}

/* No reviews message styling */
.no-reviews {
  text-align: center;
  font-family: 'Sigmar', cursive;
  font-size: 1.8rem;
  color: #ff4500;
}
</style>

