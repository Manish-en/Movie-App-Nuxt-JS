<template>
    <div class="page-container">
      <NuxtLink to="/">
        <button class="btn">Back</button>
      </NuxtLink>
  
      <div class="movie-detail">
        <div class="about">
          <div class="info">
            <div class="imgblock">
              <img :src="image" alt="Movie Poster" />
            </div>
            <div class="description">
              <h1>{{ movie.original_title }}</h1>
              <p>Status: {{ movie.status }}</p>
              <p>Released: {{ movie.release_date }}</p>
              <p>Budget: {{ movie.budget }}</p>
              <p>Language: {{ movie.original_language }}</p>
              <p>Popularity: {{ movie.popularity }}</p>
              <p>Runtime: {{ movie.runtime }} min</p>
              <p class="overview">{{ movie.overview }}</p>
              <button class="download-btn" disabled>Download</button>
            </div>
          </div>
          <!-- Placeholder for the MovieReview Component -->
          <div class="review-placeholder">
            <MovieReview :id="movie.id" />
          </div>
        </div>
      </div>
    </div>
</template>
  
<script setup>
import store from 'store2'

useHead({
    title:'Movie Details'
})

const { movie} = defineProps(['movie'])

let image = `https://image.tmdb.org/t/p/w500//${ movie.backdrop_path}`;
if(movie.backdrop_path == null){
    image = 'https://st3.depositphotos.com/1322515/35964/v/600/depositphotos_359648638-stock-illustration-image-available-icon.jpg';
}



store.set('movieId',movie.id);

</script>

<style scoped>
/* Container for the entire page */
.page-container {
  background: linear-gradient(135deg, #2c3e50, #4ca1af); /* Dark, cinematic gradient */
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 1rem;
  box-sizing: border-box;
}

/* Back button styling */
.btn {
  background-color: #e74c3c; /* Vibrant color for visibility */
  color: white;
  border: 1px solid #e74c3c;
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 8px;
  margin-bottom: 1rem;
  transition: background-color 0.3s, border-color 0.3s;
}

.btn:hover {
  background-color: #c0392b; /* Darker shade on hover */
  border-color: #c0392b;
}

/* Main container for the movie details and review section */
.movie-detail {
  display: flex;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  flex-grow: 1;
}

/* About section which includes movie info and image */
.about {
  display: flex;
  flex-grow: 1;
  gap: 1rem;
  background: rgba(0, 0, 0, 0.7); /* Semi-transparent background */
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5); /* Subtle shadow for depth */
  padding: 1rem;
}

/* Flexbox for image and description */
.info {
  display: flex;
  flex-direction: column;
  width: 70%; /* Reserve 70% for the main movie details */
  gap: 1.5rem;
}

.imgblock {
  width: 100%;
  margin: 0 auto;
  border-radius: 12px;
  overflow: hidden;
}

img {
  width: 100%;
  height: auto;
  border-radius: 12px;
  transition: transform 0.3s;
}

img:hover {
  transform: scale(1.05); /* Slight zoom on hover */
}

/* Movie description styling */
.description {
  background-color: #1c1c1c; /* Dark background for text contrast */
  padding: 1.5rem;
  border-radius: 12px;
  color: white;
  text-align: left;
  flex-grow: 1;
}

h1 {
  font-weight: bold;
  font-size: 2.5rem;
  color: #ecf0f1; /* Light color for the movie title */
  margin-bottom: 0.5rem;
}

p {
  color: #bdc3c7; /* Softer color for other text */
  margin: 0.5rem 0;
}

/* Overview paragraph styling */
.overview {
  font-size: 1.2rem;
  color: #ecf0f1;
  margin-top: 1rem;
  line-height: 1.6;
  text-align: justify;
}

/* Download button styling */
.download-btn {
  background-color: #555;
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 8px;
  cursor: not-allowed;
  margin-top: 1rem;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.download-btn:disabled {
  background-color: #333;
}

/* Placeholder for the MovieReview component */
.review-placeholder {
  width: 30%; /* Reserve 30% width for the movie review section */
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Responsive design adjustments */
@media (max-width: 768px) {
  .movie-detail {
    flex-direction: column;
  }

  .info,
  .review-placeholder {
    width: 100%;
  }

  .review-placeholder {
    margin-top: 1rem;
  }
}
</style>



